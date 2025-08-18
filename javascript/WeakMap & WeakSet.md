# WeakMap & WeakSet

`WeakMap` and `WeakSet` are special types of collections introduced in ES6 that hold "weak" references to their elements. Their "weakness" refers to how they interact with JavaScript's garbage collection mechanism.

---
## WeakMap

A WeakMap is a collection of key-value pairs where keys must be objects (or non-registered Symbols), and the references to these keys are weak.

* **Weak Keys**: If there are no other references to a key object besides the one in the `WeakMap`, that key object can be garbage collected. When a key is garbage collected, its corresponding value is also automatically removed from the `WeakMap`.
* **No Iteration**: `WeakMap`s are not iterable (`for...of` is not supported), and you cannot clear them or get their size. You can only `set`, `get`, `has`, and `delete` specific entries.
* **Use Cases**: Primarily used for associating private data or metadata with objects without preventing those objects from being garbage collected. This is common for internal caches or extending objects with data without modifying them directly.

```js
let obj1 = { id: 1 };
const weakMap = new WeakMap();
weakMap.set(obj1, "some data");

console.log(weakMap.get(obj1)); // "some data"

obj1 = null; // Remove the strong reference to obj1
// At some point, obj1 will be garbage collected, and its entry in weakMap will be removed.
// We cannot directly check if it's gone due to no iteration/size methods.
```

---
## WeakSet

A WeakSet is a collection of unique objects where the references to the objects are weak.

* **Weak References**: Similar to `WeakMap`, if an object stored in a `WeakSet` has no other strong references, it can be garbage collected. When the object is garbage collected, it's automatically removed from the `WeakSet`.
* **No Iteration**: `WeakSet`s are not iterable, and you cannot clear them or get their size. You can only `add`, `has`, and `delete` specific objects.
* **Use Cases**: Useful for tracking objects (e.g., keeping track of which objects have been "seen" or "processed") without preventing them from being garbage collected when they are no longer needed elsewhere in the application. Often used for marking objects or managing membership in a non-intrusive way.

```js
let user1 = { name: "Alice" };
const processedUsers = new WeakSet();
processedUsers.add(user1);

console.log(processedUsers.has(user1)); // true

user1 = null; // Remove the strong reference to user1
// user1 can now be garbage collected, and will automatically be removed from processedUsers.
```

The "weakness" of `WeakMap` and `WeakSet` makes them suitable for scenarios where you want to associate data or track objects without creating memory leaks by preventing objects from being garbage collected. They are not meant for general-purpose data storage where you need to iterate over all elements or know the collection's size.

---

#javascript