# Event Loop

JS (in NodeJS and browsers) runs code in a single thread, but it can handle many things “at the same time” thanks to the event loop.

> **NOTE**: The event loop is essentially a scheduler that picks tasks from different queues and runs them in order.

---
## The Queues

There are 3 types of queues that are relevant here:

1. `process.nextTick` queue

- Highest priority
- Runs after the current operation finishes, but before anything else — even before Promises.
- Used for tiny, immediate callbacks.

2. Microtask queue

- Contains callbacks from Promises (`.then`) and `queueMicrotask`.
- Runs after `nextTick` but before macrotasks.
- Used for high-priority but not immediate callbacks.

3. Macrotasks queue

- Contains `setTimeout`, `setInterval`, and I/O callbacks.
- Runs after both `nextTick` and microtasks are cleared.
- This queue runs “in the next turn of the event loop”.

---

#nodejs