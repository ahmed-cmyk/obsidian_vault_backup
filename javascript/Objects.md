# Objects 

Objects in JavaScript are fundamental data structures that store collections of key-value pairs. Properties can be defined, accessed, modified, and deleted using various methods.

Objects are commonly defined using object literals, which provide a concise syntax for creating single objects. Properties are specified as key: value pairs.

```js
const user = {
    name: "Alice",
    age: 30,
    "is Student": false // Property names with spaces require quotes
};
```

Examples:
* `{}`, `[]`, `function(){}`, `Date`, `RegExp`

For creating multiple objects with a similar structure, **constructor functions** or **ES6 Class syntax** are used, which define blueprints for objects.

```js
// Constructor Function
function Person(name, age) {
    this.name = name;
    this.age = age;
}
const person1 = new Person("Bob", 25);

// ES6 Class Syntax
class Car {
    constructor(make, model) {
        this.make = make;
        this.model = model;
    }
}
const myCar = new Car("Toyota", "Camry");
```

> **NOTE**: `Object.create()` can also be used to create a new object with a specified prototype object.

---
## Properties

### Accessing Properties
Properties can be accessed using two primary notations:

* **Dot Notation**: Used when the property name is a valid JavaScript identifier and is known beforehand.

  ```javascript
  console.log(user.name); // "Alice"
  ```

* **Bracket Notation**: Essential when the property name is dynamic (e.g., stored in a variable) or contains special characters (like spaces or hyphens).

  ```javascript
  const propName = "age";
  console.log(user[propName]);    // 30
  console.log(user["is Student"]); // false
  ```

### Adding or Updating Properties
Properties are added or updated by simply assigning a value to them using either dot or bracket notation. If the property already exists, its value is updated; otherwise, a new property is created.

```javascript
user.city = "New York"; // Adds a new property 'city'
user.age = 31;         // Updates the existing 'age' property
user["is Student"] = true; // Updates 'is Student'
```

**Example:**

```js
let obj1 = { value: 10 };
let obj2 = obj1;
obj2.value = 20;
console.log(obj1.value); // 20
```

### Deleting Properties

The delete operator is used to remove a property from an object.

```js
delete user.city; // Removes the 'city' property
```

### Checking for Property Existence

The `in` operator checks if a property exists on an object or its prototype chain.

```js
console.log("name" in user); // true
console.log("toString" in user); // true (inherited from Object.prototype)
```

The `hasOwnProperty()` method checks if a property exists directly on the object itself (not inherited).

```js
console.log(user.hasOwnProperty("name")); // true
console.log(user.hasOwnProperty("toString")); // false
```

---
### Property Descriptors (`writable`, `configurable`, `enumerable`)

Every property in a JavaScript object has a set of hidden attributes, known as **Property Descriptors**, that define its characteristics and behavior. These attributes can be inspected using `Object.getOwnPropertyDescriptor()` and modified using `Object.defineProperty()`.

**There are two types of property descriptors:**

1. **Data Descriptors**: For properties that simply hold a value.

   * `value`: The actual value of the property.
   * `writable`: A boolean indicating whether the `value` of the property can be changed (reassigned). If `false`, attempts to reassign will be ignored in non-strict mode and throw a `TypeError` in strict mode.
   * `enumerable`: A boolean indicating whether the property will show up during enumeration (e.g., in `for...in` loops or `Object.keys()`).
   * `configurable`: A boolean indicating whether the property can be deleted, and whether its attributes (except `writable` if `writable` is `false`) can be changed. Setting `configurable: false` is a one-way operation.

2. **Accessor Descriptors**: For properties defined by getter and/or setter functions.

   * `get`: A function that is called when the property is read.
   * `set`: A function that is called when the property is assigned a value.
   * `enumerable`: Same as for data descriptors.
   * `configurable`: Same as for data descriptors.
⠀
#### Example of Object.defineProperty()

When you define a property using Object.defineProperty(), if you omit writable, configurable, or enumerable, their default value is false. When properties are created via object literals or direct assignment, these attributes default to true.

```js
const myObject = {};
Object.defineProperty(myObject, 'id', {
    value: 1,
    writable: false,     // Value cannot be changed
    configurable: false, // Cannot be deleted or reconfigured
    enumerable: true     // Will appear in loops
});

console.log(myObject.id); // 1
// myObject.id = 2; // In strict mode, this would throw a TypeError
// delete myObject.id; // In strict mode, this would throw a TypeError
```

---
## Object Destructuring:

> **NOTE**: Destructuring is an ES6 feature.

You can extract properties from an object by matching their property names. The order of properties does not matter.

```js
const person = {
    name: "Alice",
    age: 30,
    city: "New York"
};

// Basic destructuring
const { name, age } = person;
console.log(name); // "Alice"
console.log(age);  // 30

// Destructuring with new variable names (aliases)
const { name: personName, city: personCity } = person;
console.log(personName); // "Alice"
console.log(personCity); // "New York"

// Default values for missing properties
const { country = "USA", age: pAge } = person;
console.log(country); // "USA"
console.log(pAge);    // 30

// Nested object destructuring
const user = {
    id: 1,
    info: {
        email: "user@example.com",
        preferences: { theme: "dark" }
    }
};
const { info: { email, preferences: { theme } } } = user;
console.log(email); // "user@example.com"
console.log(theme); // "dark"
```

---
## `Object.is()`

* Similar to `===` but:

  * `Object.is(NaN, NaN)` → `true`
  * `Object.is(+0, -0)` → `false`

**Example:**

```js
Object.is(NaN, NaN);   // true
Object.is(+0, -0);     // false
```

---
## `Object.freeze` and `Object.seal`

These two `Object` methods provide higher-level control over the extensibility and modifiability of objects, affecting all existing properties simultaneously.

### `Object.seal(obj)`

This method seals an object, meaning it prevents new properties from being added and existing properties from being deleted. However, it allows the values of existing properties to be changed, provided they are writable. When an object is sealed, the configurable attribute of all its existing properties is set to false.

```js
const sealedObj = { a: 1, b: 2 };
Object.seal(sealedObj);

sealedObj.c = 3;       // Ignored in non-strict mode, TypeError in strict mode (cannot add)
delete sealedObj.a;    // Ignored in non-strict mode, TypeError in strict mode (cannot delete)
sealedObj.a = 10;      // Allowed (value can be changed if writable)
console.log(sealedObj); // { a: 10, b: 2 }
```

### `Object.freeze(obj)`

This method freezes an object, making it immutable at its top level. When an object is frozen:

* New properties **cannot be added**.
* Existing properties **cannot be deleted**.
* The **values of existing properties cannot be changed**.
* The `writable` and `configurable` attributes of all existing properties are set to `false`.

`Object.freeze()` provides the highest level of immutability for an object itself. It's important to note that `Object.freeze()` performs a **shallow freeze**; if the object contains nested objects, those nested objects are *not* frozen and can still be modified.

```js
const frozenObj = { a: 1, b: 2, nested: { x: 10 } };
Object.freeze(frozenObj);

frozenObj.c = 3;       // Ignored/TypeError (cannot add)
delete frozenObj.a;    // Ignored/TypeError (cannot delete)
frozenObj.a = 10;      // Ignored/TypeError (cannot change value)

frozenObj.nested.x = 20; // Allowed! (nested object is not frozen)
console.log(frozenObj); // { a: 1, b: 2, nested: { x: 20 } }
```

---


#javascript