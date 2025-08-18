# Shallow vs Deep Copy Techniques

When copying objects or arrays, understanding the distinction between shallow and deep copies is crucial, as it determines how modifications to the copy affect the original data structure, especially when dealing with nested elements.

## Shallow Copy

A **Shallow Copy** creates a new object or array, but it only copies the *top-level* properties or elements. If the original object or array contains nested objects or arrays, only their *references* are copied, not the nested structures themselves. 

This implies that the copy and the original will share the same underlying nested objects. Consequently, modifying a nested object within the shallow copy will also affect the original object, and vice-versa.

### Common Shallow Copy Methods

#### For Objects

* Spread syntax: `const copiedObj = { ...originalObj };`
* `Object.assign({}, originalObj);`

#### For Arrays

* Spread syntax: `const copiedArr = [...originalArr];`
* `Array.prototype.slice()`: `originalArr.slice();`
* `Array.from()`: `Array.from(originalArr);`
* `Array.prototype.concat()`: `[].concat(originalArr);`

```js
const original = {
    name: "Alice",
    address: { city: "NY", zip: "10001" }
};
const shallowCopy = { ...original };

shallowCopy.name = "Bob"; // Modifies top-level primitive, original unaffected
shallowCopy.address.city = "LA"; // Modifies nested object, ORIGINAL IS AFFECTED
console.log(original.name);    // "Alice"
console.log(original.address.city); // "LA" - Affected!
```

## Deep Copy

A **Deep Copy** creates a completely independent duplicate of the original object or array, including all nested objects and arrays. Every level of the data structure is duplicated, ensuring that changes to the deep copy will *not* affect the original, and vice-versa.

### Common Deep Copy Methods

#### `JSON.parse(JSON.stringify(originalObject))`

* This is a simple and common method for deep copying objects/arrays that contain only JSON-compatible data (strings, numbers, booleans, `null`, plain objects, and arrays).

* **Limitations**: It **does not handle** `undefined`, `NaN`, `Infinity`, `Date` objects (converts them to strings), `RegExp`, `Map`, `Set`, `Function`, or circular references. It also breaks the prototype chain. Use with caution for complex objects.

```js
const original = { name: "Alice", address: { city: "NY" } };
const deepCopy = JSON.parse(JSON.stringify(original));

deepCopy.address.city = "LA";
console.log(original.address.city); // "NY" - Original unaffected
```

#### `structuredClone()`(Modern JavaScript)

* Introduced in modern environments (browsers, Node.js), `structuredClone()` is the most robust built-in method for deep copying. It correctly handles various built-in types like `Date`, `RegExp`, `Map`, `Set`, `ArrayBuffer`, typed arrays, and even circular references.

* **Limitations**: It cannot clone functions, DOM nodes, error objects, or properties that are accessor properties (getters/setters).

```js
const original = {
    name: "Alice",
    address: { city: "NY" },
    birthDate: new Date()
};
const deepCopy = structuredClone(original);

deepCopy.address.city = "LA";
deepCopy.birthDate.setFullYear(2000);
console.log(original.address.city); // "NY"
console.log(original.birthDate.getFullYear()); // Original year
```

### External Libraries (e.g., Lodash's `_.cloneDeep()`)

For highly complex scenarios involving functions, circular references, or other non-standard data types that `structuredClone()` cannot handle, using a battle-tested deep cloning utility from a library like Lodash (`_.cloneDeep()`) is often the most reliable solution.

---

#javascript