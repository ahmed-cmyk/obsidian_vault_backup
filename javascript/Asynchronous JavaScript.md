# Asynchronous JavaScript

Asynchronous JavaScript is crucial for handling operations that take time (like network requests, file I/O, or timers) without blocking the main execution thread. It allows your program to continue running while waiting for these operations to complete.

---
## Event Loop: Call Stack, Microtasks, Macrotasks

The **Event Loop** is the core mechanism that enables asynchronous behavior in JavaScript. It continuously monitors the Call Stack and various queues to determine what code to execute next.

### Call Stack (Execution Stack)

* A LIFO (Last-In, First-Out) stack that holds the currently executing function calls.
* When a function is called, it's pushed onto the stack. When it returns, it's popped off.
* JavaScript is single-threaded, meaning only one operation can be processed at a time on the call stack. Synchronous code executes here.

### Web APIs / Node.js APIs

* Browser/Node.js environments provide APIs (e.g., `setTimeout`, `fetch`, DOM events) that handle asynchronous operations.
* When an asynchronous function is called, it's handed off to these APIs, which run in the background (outside the JavaScript engine's main thread).

### Callback Queue (Task Queue / Macrotask Queue)

* Once an asynchronous operation (like a `setTimeout` timer expiring or a network request completing) finishes its work in the Web API, its associated callback function is placed into the **Callback Queue**.
* The Event Loop constantly checks if the Call Stack is empty. If it is, it takes the first callback from the Callback Queue and pushes it onto the Call Stack for execution. These are also known as **Macrotasks**.

### Microtask Queue

* A higher-priority queue than the Callback Queue.
* Callbacks from **Promises (`.then()`, `.catch()`, `.finally()`)** and `queueMicrotask()` are placed into the Microtask Queue.
* The Event Loop processes *all* microtasks in the Microtask Queue *before* picking up the next macrotask from the Callback Queue, as long as the Call Stack is empty. This means microtasks have priority over macrotasks.

---
### Event Loop Cycle

1. Execute all synchronous code on the Call Stack.
2. When the Call Stack is empty, the Event Loop checks the Microtask Queue.
3. Execute all microtasks in the Microtask Queue, pushing them onto the Call Stack one by one.
4. After the Microtask Queue is empty, the Event Loop checks the Callback Queue (Macrotask Queue).
5. Take one (the oldest) macrotask from the Callback Queue and push it onto the Call Stack for execution.
6. Repeat from step 2.

> **NOTE**: This continuous cycle ensures that non-blocking asynchronous operations are handled efficiently without freezing the user interface.

---
## Promises: `.then`, `.catch`, `.finally`

**Promises** are objects that represent the eventual completion (or failure) of an asynchronous operation and its resulting value. They provide a more structured and manageable way to handle asynchronous code compared to traditional callback-based approaches (which can lead to "callback hell").

**A Promise can be in one of three states:**

* **Pending**: Initial state, neither fulfilled nor rejected.
* **Fulfilled (Resolved)**: The operation completed successfully.
* **Rejected**: The operation failed.

Promises provide methods to attach handlers for different outcomes:

### `.then(onFulfilled, onRejected)`

* Used to register callbacks to be executed when the Promise is either fulfilled or rejected.
* `onFulfilled`: A function called if the Promise is resolved. It receives the resolved value as an argument.
* `onRejected`: (Optional) A function called if the Promise is rejected. It receives the rejection reason (error) as an argument.
* `.then()` always returns a **new Promise**, allowing for chaining.

```js
myPromise
    .then(value => {
        console.log("Promise fulfilled with:", value);
        return value * 2; // Returns a new Promise resolving with value * 2
    })
    .then(newValue => {
        console.log("Chained then, new value:", newValue);
    });
```

### `.catch(onRejected)`

* A shorthand for `.then(null, onRejected)`. It's specifically used to handle errors (rejections) in a Promise chain.
* It catches errors from any preceding Promise in the chain.
* Also returns a new Promise.

```js
myPromise
    .then(value => {
        throw new Error("Something went wrong!"); // This will be caught by .catch
    })
    .catch(error => {
        console.error("Promise rejected with:", error.message);
    });
```

### `.finally(onFinally)`

* Registers a callback that is executed regardless of whether the Promise was fulfilled or rejected.
* The `onFinally` callback does not receive any arguments.
* It's useful for performing cleanup operations (e.g., hiding a loading spinner).
* It also returns a new Promise, passing through the original Promise's resolution or rejection value.

```js
myPromise
    .then(value => console.log("Success:", value))
    .catch(error => console.error("Failure:", error))
    .finally(() => {
        console.log("Promise finished (cleanup done).");
    });
```

> **NOTE**: Promise chaining is a powerful pattern for sequencing asynchronous operations, where the return value of one `.then()` handler becomes the input for the next.

---
## How `async` / `await` works

`async` and `await` are ES2017 features that provide a more synchronous-looking syntax for working with Promises, making asynchronous code easier to read and write. They are essentially syntactic sugar over Promises.

### **`async`keyword**

* Used to declare an asynchronous function.
* An `async` function always returns a **Promise**. If the function returns a non-Promise value, it's automatically wrapped in a resolved Promise. If it throws an error, it's wrapped in a rejected Promise.

```js
async function fetchData() {
    return "Data fetched!"; // Returns Promise.resolve("Data fetched!")
}
fetchData().then(data => console.log(data)); // Data fetched!
```

### `await` keyword

* Can only be used *inside* an `async` function.
* It pauses the execution of the `async` function until the Promise it's waiting for settles (either resolves or rejects).
* If the Promise resolves, `await` returns the resolved value.
* If the Promise rejects, `await` throws the rejected value (error), which can then be caught using a `try...catch` block within the `async` function.

```js
async function getUserData(userId) {
    try {
        const response = await fetch(`https://api.example.com/users/${userId}`); // Pause until fetch resolves
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        const data = await response.json(); // Pause until json parsing resolves
        console.log("User data:", data);
        return data;
    } catch (error) {
        console.error("Failed to fetch user data:", error);
        // Re-throw or return a default value
        throw error;
    }
}

getUserData(123);
```

> **NOTE**: `async`/`await` simplifies complex Promise chains, making asynchronous code look and behave more like synchronous code, which improves readability and maintainability.

---
## Why `setTimeout(fn, 0)` doesn’t execute immediately

Even when `setTimeout` is called with a delay of `0` milliseconds (`setTimeout(fn, 0)`), the provided function (`fn`) does not execute immediately. This is due to the nature of the Event Loop and how asynchronous tasks are prioritized.

When `setTimeout(fn, 0)` is called:

1. The `fn` callback is handed off to the **Web API** (or Node.js timer module).
2. The timer (even if 0ms) is set.
3. Once the timer expires (which is almost immediately for 0ms), the `fn` callback is placed into the **Callback Queue (Macrotask Queue)**.
4. The JavaScript Event Loop will only pick up `fn` from the Callback Queue and push it onto the Call Stack for execution *after* the Call Stack is completely empty of all currently executing synchronous code, and *after* the Microtask Queue has been fully processed.

⠀
Therefore, `setTimeout(fn, 0)` effectively schedules the function to run **as soon as possible, but not synchronously and not necessarily before other pending microtasks**. It ensures the function runs in a separate "turn" of the event loop, after the current synchronous execution block and any pending microtasks have completed.

```js
console.log("Start");

setTimeout(() => {
    console.log("setTimeout callback"); // This runs after "End" and any microtasks
}, 0);

Promise.resolve().then(() => {
    console.log("Promise microtask"); // This runs before setTimeout callback
});

console.log("End");

// Expected output:
// Start
// End
// Promise microtask
// setTimeout callback
```

---
## Difference between Microtask Queue & Callback Queue

Both the Microtask Queue and the Callback Queue (also known as the Macrotask Queue or Task Queue) hold callbacks waiting to be executed by the Event Loop. The key difference lies in their **priority** and **processing order**.

### Callback Queue (Macrotask Queue)

* **Priority**: Lower priority.
* **Contents**: Holds callbacks for traditional asynchronous operations like `setTimeout()`, `setInterval()`, I/O operations (e.g., network requests like `fetch` once they complete), and UI rendering events.
* **Processing**: The Event Loop processes **one** macrotask from this queue per turn of the loop. After a macrotask completes, the Event Loop checks the Microtask Queue before moving to the next macrotask.

### Microtask Queue

* **Priority**: Higher priority.
* **Contents**: Holds callbacks for Promises (`.then()`, `.catch()`, `.finally()`) and `queueMicrotask()`. `MutationObserver` callbacks also go here.
* **Processing**: The Event Loop processes **all** microtasks in this queue until it is empty, *before* moving on to the next macrotask from the Callback Queue. This "drainage" of the microtask queue happens after each macrotask completes and after the initial synchronous code execution.

---
## Execution Order Summary

1. Synchronous code on the Call Stack.
2. All pending Microtasks (until the Microtask Queue is empty).
3. One Macrotask (from the Callback Queue).
4. Repeat from step 2 (drain all Microtasks, then one Macrotask, etc.).

> **NOTE**: This priority system ensures that Promise-based operations, which are often critical for immediate follow-up actions, are handled more quickly than other asynchronous tasks like timers or UI events.

---

#javascript