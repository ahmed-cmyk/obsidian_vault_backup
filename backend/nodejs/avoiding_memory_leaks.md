# Avoiding Memory Leaks

Although GC is automatic, keeping unnecessary references prevents memory from being freed. This is called a **memory leak**.

### Common Causes and Solutions

**Global Variables**

* Problem: Declaring variables without `let`/`const` makes them global.
* Solution: Always declare properly.

```js
// Bad
foo = 'something';

// Good
let foo = 'something';
```

**Forgotten Timers and Callbacks**

* Problem: Unstopped `setInterval`/`setTimeout` or event listeners keep references alive.
* Solution: Always clear them when done.

```js
const id = setInterval(doSomething, 1000);
// Later
clearInterval(id);

element.removeEventListener('click', handler);
```

**Detached DOM Elements**

* Problem: Removing DOM elements from the page but keeping JS references.
* Solution: Remove references.

```js
document.body.removeChild(div);
div = null;
```

**Closures Holding References**

* Problem: Long-lived closures retain data unnecessarily.
* Solution: Only capture what is needed in closures.

```js
function create() {
    const bigData = new Array(1000000).fill('*');
    return function() {
        console.log(bigData.length);
    };
}

const leaky = create(); // bigData stays alive
```

**Unbounded Caches**

* Problem: Caches grow indefinitely.
* Solution: Prune unused data or use `WeakMap`/`WeakSet` for ephemeral keys.

---

# Tools to Detect Memory Leaks

* **Chrome DevTools → Memory tab**

  * Take heap snapshots and compare them over time.
  * Record allocations and find what grows unexpectedly.

* **Node.js `--inspect`**

  * Run Node with `--inspect` or `--inspect-brk` and connect to Chrome DevTools.

* **heapdump (Node module)**

  * Capture heap snapshots programmatically for offline analysis.

* **Valgrind / other profilers**

  * Lower-level tools if needed, but rarely necessary for JS itself.

---

# Steps to Debug Memory Leaks

1. Reproduce the problem in a controlled environment.
2. Open Chrome DevTools → Memory tab or attach to Node with `--inspect`.
3. Take an initial **heap snapshot**.
4. Perform actions that you suspect cause the leak.
5. Take another **heap snapshot**.
6. Compare snapshots: look for objects that keep growing and are not freed.
7. Use the **Retainers** panel to find what is keeping them alive.
8. Fix the code (remove listeners, clear intervals, nullify references, etc.).
9. Repeat the process to confirm the fix.

---
## Common Memory Leak Patterns to Look Out For

Even with garbage collection, certain coding patterns can unintentionally keep data in memory. Here are some of the most common ones you should audit for:

---

### 1. Unnecessary Global Variables

* Variables declared without `let`/`const` become properties of the global object.
* They persist for the lifetime of the application.

#### How to Fix

* Always declare variables properly.
* Minimize use of global scope; encapsulate code in modules or functions.

---

### 2. Forgotten Timers & Intervals

* `setInterval` callbacks keep executing and retain any captured data.
* `setTimeout` can also keep references alive until it runs.

#### How to Fix

* Always call `clearInterval()` or `clearTimeout()` when done.
* Ensure cleanup happens during teardown of components/pages.

---

### 3. Unremoved Event Listeners

* Event listeners attached to DOM elements or global objects stay active until explicitly removed.

#### How to Fix

* Remove event listeners with `removeEventListener` when no longer needed.
* In frameworks (React, Vue), use lifecycle hooks to clean up listeners.

---

### 4. Detached DOM Elements

* Keeping references to DOM nodes after removing them from the document prevents them from being collected.

#### How to Fix

* Set JS variables referencing removed DOM elements to `null`.

---

### 5. Growing Caches or Data Structures

* Maps, Sets, arrays, or objects used as caches can grow indefinitely if old data is not cleaned up.

#### How to Fix

* Regularly prune stale or unused data.
* Use `WeakMap` or `WeakSet` for keys tied to objects that should not prevent GC.

---

### 6. Long-Lived Closures

* Closures can keep variables alive longer than intended, especially when passed around or stored.

#### How to Fix

* Avoid capturing large objects in closures unless necessary.
* Ensure closures don’t outlive the context in which they’re needed.

---

### 7. Overly Long-Lived Observables / Subscriptions

* In reactive programming (RxJS, EventEmitters), subscriptions keep memory alive until unsubscribed.

#### How to Fix

* Always unsubscribe from observables or event streams when done.

---

### 8. Large Inline Data on Global Objects

* Attaching large data blobs directly to `window`, `document`, or global namespaces keeps them alive indefinitely.

#### How to Fix

* Avoid attaching unnecessary data to global objects.

---

### General Advice

* Audit code regularly, especially long-running servers or SPAs.
* Automate heap snapshots in CI/CD for baseline tracking.
* Use tools like Chrome DevTools, Node’s `--inspect`, or `heapdump` for analysis.

---

#nodejs