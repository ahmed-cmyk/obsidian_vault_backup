# `__proto__`, `prototype`, and Constructor Functions

These are core concepts in JavaScript’s inheritance model.

---
## `__proto__`

- Internal reference of an object to its prototype.
- Non-standard (but widely supported) way to access the prototype.
- Preferred: use `Object.getPrototypeOf()` and `Object.setPrototypeOf()` instead.

```js
const myObject = {};
console.log(myObject.__proto__ === Object.prototype); // true
```

---
## Prototype (on functions)

A property of functions (not objects) that is used when creating new objects with new.
Any object created by a constructor function inherits from that function’s prototype.

```js
function Person(name) {
    this.name = name;
}
Person.prototype.greet = function () {
    console.log(`Hello, ${this.name}`);
};

const alice = new Person("Alice");
console.log(alice.__proto__ === Person.prototype); // true
alice.greet(); // Hello, Alice
```

---
## Constructor Functions

Functions used with new to create objects.

Steps:
- Create empty object
- Set its __proto__ to constructor’s prototype
- Run constructor with this bound to new object
- Return the object

```js
function Person(name) {
    this.name = name;
}
const bob = new Person("Bob");
```

---

#javascript