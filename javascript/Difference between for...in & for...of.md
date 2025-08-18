# Difference between `for...in` & `for...of`

These two `for` loops are used for iteration, but they iterate over different types of properties/values.

---
## `for...in`loop

* Iterates over the **enumerable string properties** of an object, including inherited enumerable properties from the prototype chain.
* It iterates over *keys* (property names).

> **NOTE**: **Not recommended for iterating over arrays** because it can iterate over properties that are not numeric indices (e.g., methods added to `Array.prototype`), and the order of iteration is not guaranteed for object properties.

```js
const myObject = { a: 1, b: 2, c: 3 };
for (const key in myObject) {
    console.log(`${key}: ${myObject[key]}`);
}
// Output:
// a: 1
// b: 2
// c: 3

const myArray = [10, 20, 30];
myArray.customProp = "hello";
for (const index in myArray) {
    console.log(index); // "0", "1", "2", "customProp" (can include non-numeric keys)
}
```

---
## `for...of` loop

* Introduced in ES6, it iterates over the **values of iterable objects** (objects that implement the `Symbol.iterator` method). This includes built-in iterables like `Array`, `String`, `Map`, `Set`, `NodeList`, and custom iterables.
* It iterates over *values*.

> **NOTE**: **The preferred way to iterate over arrays** and other iterable collections, as it guarantees correct order and only iterates over the actual elements.

```js
const myArray = [10, 20, 30];
for (const value of myArray) {
    console.log(value); // 10, 20, 30
}

const myString = "abc";
for (const char of myString) {
    console.log(char); // "a", "b", "c"
}

const mySet = new Set([1, 2, 3]);
for (const item of mySet) {
    console.log(item); // 1, 2, 3
}
```

In summary, use `for...of` for iterating over the values of arrays and other iterable collections, and `for...in` (with caution and often `hasOwnProperty` checks) for iterating over object property names.

---

#javascript