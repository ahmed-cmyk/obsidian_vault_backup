# Rust Web Server Project

## Building a Single-Threaded Web Server

Start by creating a basic server that listens for TCP connections and responds to HTTP requests.

```rust
use std::io::prelude::*;
use std::net::TcpListener;
use std::net::TcpStream;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();

        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];
    stream.read(&mut buffer).unwrap();

    let get = b"GET / HTTP/1.1\r\n";

    let (status_line, filename) = if buffer.starts_with(get) {
        ("HTTP/1.1 200 OK", "hello.html")
    } else {
        ("HTTP/1.1 404 NOT FOUND", "404.html")
    };

    let contents = std::fs::read_to_string(filename).unwrap();

    let response = format!(
        "{}\
\
Content-Length: {}\
\
\
\
{}",
        status_line,
        contents.len(),
        contents
    );

    stream.write(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}
```

- **`TcpListener`**: Listens for incoming TCP connections.
- **`TcpStream`**: Represents an open connection between a client and a server.
- **HTTP Request Parsing**: Basic parsing of the request line.
- **HTTP Response**: Constructing a simple HTTP response with status line, headers, and body.

## Making the Server Multithreaded with a Thread Pool

The single-threaded server blocks on slow requests. To handle multiple requests concurrently, you can use a thread pool.

### Thread Pool Implementation

```rust
use std::sync::mpsc;
use std::sync::Arc;
use std::sync::Mutex;
use std::thread;

// ... (handle_connection function from above)

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    let pool = ThreadPool::new(4);

    for stream in listener.incoming() {
        let stream = stream.unwrap();

        pool.execute(|| {
            handle_connection(stream);
        });
    }
}

pub struct ThreadPool {
    workers: Vec<Worker>,
    sender: mpsc::Sender<Job>,
}

type Job = Box<dyn FnOnce() + Send + 'static>;

impl ThreadPool {
    pub fn new(size: usize) -> ThreadPool {
        assert!(size > 0);

        let (sender, receiver) = mpsc::channel();

        let receiver = Arc::new(Mutex::new(receiver));

        let mut workers = Vec::with_capacity(size);

        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }

        ThreadPool {
            workers,
            sender,
        }
    }

    pub fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.send(job).unwrap();
    }
}

struct Worker {
    id: usize,
    thread: thread::JoinHandle<()>, // Store the JoinHandle
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let job = receiver.lock().unwrap().recv().unwrap();
            println!("Worker {} got a job; executing.", id);
            job();
        });

        Worker { id, thread }
    }
}
```

- **`ThreadPool`**: Manages a fixed number of worker threads.
- **`Worker`**: Each worker thread continuously fetches jobs from a queue and executes them.
- **Message Passing (`mpsc::channel`)**: Used to send `Job`s (closures) from the main thread to the worker threads.
- **Shared State (`Arc<Mutex<...>>`)**: The `receiver` is wrapped in `Arc` for multiple ownership and `Mutex` for safe shared access across threads.

## Graceful Shutdown

To ensure the server shuts down cleanly, you need to implement a way for the `ThreadPool` to signal its workers to stop and for the main thread to wait for them.

```rust
// ... (ThreadPool, Worker, Job definitions)

impl Drop for ThreadPool {
    fn drop(&mut self) {
        println!("Shutting down all workers.");

        // Drop the sender to signal workers to stop receiving jobs
        drop(self.sender.take()); // Use take() to consume the sender

        for worker in &mut self.workers {
            println!("Shutting down worker {}", worker.id);

            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}

// In main, the pool will be dropped when main exits, triggering the Drop impl
fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    let pool = ThreadPool::new(4);

    for stream in listener.incoming().take(2) { // Take only 2 requests for demonstration
        let stream = stream.unwrap();
        pool.execute(|| {
            handle_connection(stream);
        });
    }

    println!("Shutting down.");
}
```

- **`Drop` Trait**: Implement `Drop` for `ThreadPool` to define cleanup logic.
- **Signaling Workers**: Dropping the `sender` end of the channel causes `recv()` on the `receiver` end to return an error, signaling workers to exit their loop.
- **Joining Threads**: The `drop` implementation iterates through workers and calls `join()` on their threads, ensuring all jobs are completed before the program exits.

