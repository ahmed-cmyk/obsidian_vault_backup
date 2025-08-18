# Garbage Collection in JavaScript

JavaScript manages memory **automatically**, freeing memory when it determines that data is no longer needed. This is called **garbage collection (GC)**.

---

## Reachability

The key concept in JS memory management is **reachability**:

> A value is *reachable* if it can still be accessed or used from somewhere in the program.

Reachable values are kept in memory. Anything unreachable is considered “garbage” and is removed.

### Roots

Some values are **always reachable**, called *roots*. Examples:

* Local variables and parameters of the currently executing function
* Local variables and parameters of parent functions in the call stack
* Global variables

### References

If an object is referenced by a reachable value, it is also reachable.

> If you can reach it by following a chain of references from the roots, it stays alive.

If no such path exists, the value is garbage collected.

---

## How Garbage Collection Works

The most common algorithm in modern JS engines is **mark‑and‑sweep**.

### Mark‑and‑Sweep

1. Start with the *roots* and mark them as reachable.
2. Recursively mark all values that can be reached from the roots.
3. Anything left unmarked is unreachable and removed from memory.

Think of it as pouring paint over the roots — everything connected gets painted and stays; the rest is discarded.

---

## Optimizations

Modern engines use several optimizations to improve performance and minimize pauses during garbage collection:

### Generational GC

* Objects are categorized as *new* or *old*.
* Most objects die young, so the engine focuses on cleaning up new objects more frequently.
* Long‑lived (old) objects are checked less often.

### **Incremental GC**

* Instead of scanning the whole heap at once (which could pause execution noticeably), the engine splits the heap into parts and cleans them incrementally.
* Many small, fast collections instead of one big pause.

### Idle‑Time GC

* The engine tries to run GC when the CPU is idle to minimize impact on the main code execution.

---

## Notes

* GC is **automatic**, but you still need to write memory‑efficient code: avoid keeping unnecessary references, especially in long‑lived scopes.
* Exact implementation details vary between engines (e.g., V8, SpiderMonkey), and evolve over time.

---

#nodejs