# Rust Concurrency

## Threads

Threads allow you to run multiple parts of your code concurrently. Rust's standard library provides `std::thread` for creating new threads.

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("hi number {} from the main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }

    handle.join().unwrap(); // Wait for the spawned thread to finish
}
```

- **`thread::spawn`**: Creates a new thread. It takes a closure that contains the code to run in the new thread.
- **`join()`**: Blocks the current thread until the thread represented by the handle terminates.

### Using `move` Closures with Threads

When you use `thread::spawn`, the closure might need to take ownership of the values it uses from the environment. The `move` keyword forces the closure to take ownership of the values it captures.

```rust
fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(move || {
        println!("Here's a vector from the spawned thread: {:?}", v);
    });

    handle.join().unwrap();
}
```

## Message Passing Concurrency

Message passing is a concurrency model where threads communicate by sending messages to each other via channels. Rust's standard library provides `std::sync::mpsc` (multiple producer, single consumer) for this.

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel(); // Create a new channel: transmitter (tx) and receiver (rx)

    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap(); // Send a message
        // println!("val is {}", val); // This would not compile, val is moved
    });

    let received = rx.recv().unwrap(); // Receive a message
    println!("Got: {}", received);
}
```

- **Multiple Producers**: You can create multiple transmitters by cloning the original transmitter.

```rust
fn main() {
    let (tx, rx) = mpsc::channel();

    let tx1 = mpsc::Sender::clone(&tx);
    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];

        for val in vals {
            tx1.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    thread::spawn(move || {
        let vals = vec![
            String::from("more"),
            String::from("messages"),
            String::from("for"),
            String::from("you"),
        ];

        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    for received in rx.iter() {
        println!("Got: {}", received);
    }
}
```

## Shared-State Concurrency

Multiple threads can share ownership of data. Rust ensures this is done safely using smart pointers like `Arc<T>` and `Mutex<T>`.

### Mutexes (`Mutex<T>`)

A mutex (mutual exclusion) allows only one thread to access some data at any given time. To access the data inside a `Mutex`, you must first acquire a lock.

```rust
use std::sync::{Mutex, Arc};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap(); // Acquire a lock
            *num += 1; // Mutate the data
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

- **`Mutex::new(0)`**: Creates a new mutex holding the value `0`.
- **`counter.lock().unwrap()`**: Acquires a lock. This call will block the current thread until the lock is available. `unwrap()` handles potential poisoning (when a thread holding the lock panics).
- **`Arc<T>`**: Used to enable multiple threads to own the `Mutex`. `Rc<T>` is not thread-safe.

## `Send` and `Sync` Traits

Rust has two marker traits, `Send` and `Sync`, that help ensure thread safety.

- **`Send`**: A type `T` is `Send` if it is safe to send it to another thread. Almost all primitive types are `Send`. Types composed entirely of `Send` types are automatically `Send`.
- **`Sync`**: A type `T` is `Sync` if it is safe to share it between multiple threads (i.e., `&T` is `Send`). Types composed entirely of `Sync` types are automatically `Sync`.

Rust's type system and borrow checker prevent data races at compile time, making concurrency much safer.

## Asynchronous Programming with `async/await`

While not explicitly in this chapter of the book, `async/await` is a fundamental part of modern Rust concurrency for writing asynchronous code. It allows for concurrent operations without the overhead of traditional threads.

- **`async fn`**: Defines an asynchronous function that returns a `Future`.
- **`await`**: Pauses the execution of an `async` function until a `Future` completes.

```rust
// This example requires an async runtime like tokio or async-std
// Add to Cargo.toml:
// [dependencies]
// tokio = { version = "1", features = ["full"] }

async fn fetch_data() -> String {
    println!("Fetching data...");
    tokio::time::sleep(std::time::Duration::from_secs(2)).await;
    println!("Data fetched.");
    String::from("Some data")
}

async fn process_data(data: String) {
    println!("Processing data: {}", data);
}

#[tokio::main] // Macro to run the async main function
async fn main() {
    println!("Starting async operations...");
    let data = fetch_data().await; // Await the completion of fetch_data
    process_data(data).await;
    println!("Async operations finished.");
}
```

- **Futures**: Represent a value that may not be available yet. They are *lazy* and only execute when polled by an executor.
- **Executors**: A runtime that polls futures and drives them to completion (e.g., `tokio`, `async-std`).

