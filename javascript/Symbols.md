# Symbols

**Symbols** are a new primitive data type introduced in ES6. A Symbol value is a unique and immutable identifier. They are primarily used as object property keys to avoid naming collisions, especially when extending objects or working with third-party code.

---
## Key Characteristics

| Characteristic     | Description                                                  |
|--------------------|--------------------------------------------------------------|
| **Uniqueness**     | Every time you create a Symbol, it’s guaranteed to be unique, even if it has the same description. |
| **Immutability**   | Once created, a Symbol value cannot be changed.              |
| **Not Enumerable** | Symbol properties are not included in `for…in` loops, `Object.keys()`, `Object.values()`, or `Object.entries()`. They are “hidden” from typical iteration. |
| **Discoverable**   | You can find Symbol properties using `Object.getOwnPropertySymbols()` or `Reflect.ownKeys()`. |

**Uniqueness example:**

```js
const id1 = Symbol('id');
const id2 = Symbol('id');
console.log(id1 === id2); // false
```

### When to use Symbols

#### Hidden Object Properties

To add properties to an object that won't interfere with existing or future string-named properties, and won't show up during regular enumeration. This is ideal for adding metadata or private-like properties.

```js
const user = {
    name: "John"
};
const userId = Symbol('user ID');
user[userId] = 123;

console.log(user.name); // John
console.log(user[userId]); // 123

for (let key in user) {
    console.log(key); // Only "name" is logged
}
console.log(Object.keys(user)); // ["name"]
console.log(Object.getOwnPropertySymbols(user)); // [Symbol(user ID)]
```

#### Defining Constants for Events or Actions

To ensure unique identifiers for event types or action types in frameworks like Redux, preventing accidental collisions.

```js
const EVENT_CLICK = Symbol('click');
const EVENT_SUBMIT = Symbol('submit');

// Use these Symbols as unique identifiers
```

#### Well-Known Symbols (Built-in Symbols)

JavaScript has a set of built-in "well-known" Symbols (e.g., `Symbol.iterator`, `Symbol.hasInstance`, `Symbol.toStringTag`) that are used internally by the language to define or customize the behavior of objects. For example, `Symbol.iterator` defines how an object can be iterated over using `for...of`.

```js
const myIterable = {
    [Symbol.iterator]: function* () {
        yield 1;
        yield 2;
        yield 3;
    }
};
for (const val of myIterable) {
    console.log(val); // 1, 2, 3
}
```

> **NOTE**: Symbols provide a powerful tool for creating unique, non-colliding identifiers and for customizing object behavior at a low level.

---

#javascript