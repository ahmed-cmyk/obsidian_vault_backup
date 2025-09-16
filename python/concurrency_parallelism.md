# Python Concurrency and Parallelism

Concurrency and parallelism are two related but distinct concepts in programming that deal with executing multiple tasks, often to improve performance or responsiveness.

-   **Concurrency:** Deals with managing multiple tasks that *appear* to run at the same time, often by interleaving their execution on a single CPU core. It's about dealing with many things at once.
-   **Parallelism:** Deals with actually executing multiple tasks *simultaneously* on multiple CPU cores or processors. It's about doing many things at once.

Python has several modules to achieve concurrency and parallelism, each with its own use cases and limitations (especially due to the Global Interpreter Lock - GIL).

## The Global Interpreter Lock (GIL)

The GIL is a mutex that protects access to Python objects, preventing multiple native threads from executing Python bytecodes at once. This means that even on a multi-core processor, only one thread can execute Python bytecode at any given time.

**Implications of GIL:**
-   **CPU-bound tasks:** For tasks that spend most of their time doing computations (e.g., heavy number crunching), multithreading in Python will *not* lead to true parallelism and might even be slower due to GIL overhead.
-   **I/O-bound tasks:** For tasks that spend most of their time waiting for external resources (e.g., network requests, file I/O), multithreading can still be beneficial. While one thread is waiting for I/O, the GIL is released, allowing other threads to run.

To achieve true parallelism for CPU-bound tasks in Python, you typically need to use multiprocessing.

## 1. Multithreading (`threading` module)

Multithreading allows you to run multiple parts of a program concurrently within the same process. Threads share the same memory space.

### Basic Thread Creation

```python
import threading
import time

def task(name):
    print(f"Thread {name}: Starting")
    time.sleep(2) # Simulate some work (I/O bound, releases GIL)
    print(f"Thread {name}: Finishing")

# Create threads
t1 = threading.Thread(target=task, args=("One",))
t2 = threading.Thread(target=task, args=("Two",))

# Start threads
t1.start()
t2.start()

# Wait for threads to complete
t1.join()
t2.join()

print("All threads finished.")
```

### Thread Synchronization (Locks)

When multiple threads access shared resources, race conditions can occur. Locks (mutexes) are used to protect critical sections of code, ensuring that only one thread can access the shared resource at a time.

```python
import threading

shared_resource = 0
lock = threading.Lock()

def increment():
    global shared_resource
    for _ in range(100000):
        lock.acquire() # Acquire the lock
        try:
            shared_resource += 1
        finally:
            lock.release() # Release the lock (important even if error occurs)

threads = []
for _ in range(5):
    thread = threading.Thread(target=increment)
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

print(f"Final value of shared_resource: {shared_resource}") # Should be 500000
```

### `threading.local`

Provides thread-local storage. Data stored in a `threading.local()` object is specific to each thread.

```python
import threading

thread_data = threading.local()

def set_and_print_data():
    thread_data.value = threading.current_thread().name
    print(f"{threading.current_thread().name}: {thread_data.value}")

t1 = threading.Thread(target=set_and_print_data, name="Thread-A")
t2 = threading.Thread(target=set_and_print_data, name="Thread-B")

t1.start()
t2.start()

t1.join()
t2.join()
```

## 2. Multiprocessing (`multiprocessing` module)

Multiprocessing allows you to run multiple processes, each with its own Python interpreter and memory space. This bypasses the GIL, enabling true parallelism for CPU-bound tasks.

### Basic Process Creation

```python
import multiprocessing
import time

def cpu_bound_task(name):
    print(f"Process {name}: Starting CPU-bound task")
    # Simulate heavy computation
    result = sum(i*i for i in range(10**7))
    print(f"Process {name}: Finishing CPU-bound task with result {result % 100}")

# Create processes
p1 = multiprocessing.Process(target=cpu_bound_task, args=("One",))
p2 = multiprocessing.Process(target=cpu_bound_task, args=("Two",))

# Start processes
p1.start()
p2.start()

# Wait for processes to complete
p1.join()
p2.join()

print("All processes finished.")
```

### Process Communication (Queues, Pipes)

Since processes have separate memory spaces, you need explicit mechanisms for inter-process communication (IPC).

-   **`multiprocessing.Queue`:** A thread-safe and process-safe queue for passing messages between processes.

    ```python
    import multiprocessing

    def worker(q):
        data = q.get()
        print(f"Worker received: {data}")
        q.put(data * 2)

    q = multiprocessing.Queue()
    p = multiprocessing.Process(target=worker, args=(q,))
    p.start()

    q.put(10)
    result = q.get()
    print(f"Main process received: {result}")

    p.join()
    ```

-   **`multiprocessing.Pipe`:** For two-way communication between two processes.

    ```python
    import multiprocessing

    def sender(conn):
        conn.send("Hello from sender")
        print(f"Sender received: {conn.recv()}")
        conn.close()

    def receiver(conn):
        print(f"Receiver received: {conn.recv()}")
        conn.send("Hello from receiver")
        conn.close()

    parent_conn, child_conn = multiprocessing.Pipe()
    p1 = multiprocessing.Process(target=sender, args=(parent_conn,))
    p2 = multiprocessing.Process(target=receiver, args=(child_conn,))

    p1.start()
    p2.start()

    p1.join()
    p2.join()
    ```

### Process Pools (`multiprocessing.Pool`)

For distributing tasks across a pool of worker processes, `Pool` is very convenient.

```python
import multiprocessing
import time

def square(number):
    time.sleep(0.1) # Simulate some work
    return number * number

if __name__ == '__main__': # Important for multiprocessing on Windows
    numbers = range(10)
    with multiprocessing.Pool(processes=4) as pool:
        # map applies a function to each item in an iterable
        results = pool.map(square, numbers)
    print(f"Squared numbers: {results}")

    # apply_async for asynchronous results
    async_results = [pool.apply_async(square, (num,)) for num in numbers]
    final_results = [res.get() for res in async_results]
    print(f"Squared numbers (async): {final_results}")
```

## 3. Asynchronous Programming (`asyncio` module)

`asyncio` is a library to write concurrent code using the `async`/`await` syntax. It's a single-threaded, single-process approach to concurrency, ideal for I/O-bound and high-level structured network code. It achieves concurrency by switching between tasks when one task is waiting for I/O.

### Key Concepts

-   **Coroutines:** Functions defined with `async def`. They can be paused and resumed.
-   **`await`:** Used to pause the execution of a coroutine until an awaitable (another coroutine, a Future, a Task) completes.
-   **Event Loop:** The heart of `asyncio`, responsible for managing and executing coroutines.
-   **Tasks:** Wrappers around coroutines that schedule them to run in the event loop.

### Basic `asyncio` Example

```python
import asyncio
import time

async def fetch_data(delay, data):
    print(f"Fetching {data}...")
    await asyncio.sleep(delay) # Simulate I/O-bound operation
    print(f"Finished fetching {data}")
    return data

async def main():
    start_time = time.time()

    # Run tasks concurrently
    task1 = asyncio.create_task(fetch_data(2, "User Data"))
    task2 = asyncio.create_task(fetch_data(1, "Product Data"))

    # Await results (execution continues here only after tasks complete)
    user_data = await task1
    product_data = await task2

    print(f"Received: {user_data}, {product_data}")
    end_time = time.time()
    print(f"Total time: {end_time - start_time:.2f} seconds")

# Run the main coroutine
if __name__ == "__main__":
    asyncio.run(main())
# Expected output: Total time will be closer to the max delay (2 seconds), not sum (3 seconds)
```

### `asyncio.gather`

Runs multiple awaitable objects concurrently and waits for all of them to complete.

```python
import asyncio

async def say_after(delay, what):
    await asyncio.sleep(delay)
    print(what)

async def main_gather():
    await asyncio.gather(
        say_after(1, 'hello'),
        say_after(2, 'world')
    )
    print("All greetings done.")

if __name__ == "__main__":
    asyncio.run(main_gather())
```

## Choosing the Right Approach

-   **Multithreading:** Best for I/O-bound tasks where you need to wait for external resources (network, disk). Limited by GIL for CPU-bound tasks.
-   **Multiprocessing:** Best for CPU-bound tasks where you need to utilize multiple CPU cores. Each process has its own memory, so communication is more complex.
-   **`asyncio`:** Best for highly concurrent I/O-bound operations (e.g., many network requests) where you want to avoid the overhead of multiple threads/processes and prefer a single-threaded, event-driven model.
