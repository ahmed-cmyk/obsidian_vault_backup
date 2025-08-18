# setImmediate

The `setImmediate` function delays the execution of a function to be called after the current event loops finish all their execution. It's very similar to calling `setTimeout` with 0 ms delay.

---
## Execution

`setImmediate` goes into the macrotask queue meaning it waits for `process.nextTick` and then microtasks to finish.

- `setImmediate` runs after I/O events
- `setTimeout(0)` runs after the specified delay, minimum ~ 1ms in practice

> **IMPORTANT**: In Node.js, usually setImmediate() happens **before** setTimeout(0) if called during I/O. But outside I/O, their order is not guaranteed.

---
## When it’s used

When you want to execute some piece of code asynchronously, but as soon as possible, one option is to use the setImmediate() function provided by Node.js

---

#nodejs