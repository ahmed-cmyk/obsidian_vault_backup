# `this` & Context

The `this` keyword in JavaScript is a special identifier that refers to the **context** in which a function is executed. Its value is determined dynamically at runtime, based on *how* the function is called, rather than where it is defined (with the exception of arrow functions). Understanding `this` is crucial for working with objects and functions effectively.

---
## How `this` behaves in different contexts:

The value of `this` changes depending on the execution context.

### Global Context

When `this` is used outside of any function in the global scope, it refers to the **global object**.

* In web browsers, the global object is `window`.
* In Node.js, the global object is `global`.
* In strict mode, `this` in the global context still refers to the global object.

```js
console.log(this === window); // true (in browser)
```

### Object Methods

When a function is called as a method of an object (i.e., using dot notation `object.method()`), `this` inside that method refers to the **object that owns the method**. This is the most common and intuitive use of `this`.

```js
const person = {
    name: "Alice",
    greet: function() {
        console.log(`Hello, my name is ${this.name}`);
    }
};
person.greet(); // 'this' refers to 'person', logs "Hello, my name is Alice"

const anotherPerson = { name: "Bob" };
anotherPerson.greet = person.greet; // Assign the method
anotherPerson.greet(); // 'this' refers to 'anotherPerson', logs "Hello, my name is Bob"
```

### Regular Functions (Standalone Functions)

When a function is called as a standalone function (not as a method of an object), `this` typically refers to the **global object** (`window` in browsers, `global` in Node.js) in non-strict mode.

**NOTE:** In **strict mode** (`'use strict';`), `this` inside a standalone function is `undefined`. This helps prevent accidental global variable creation and makes `this` behavior more predictable.

```js
function showThis() {
    console.log(this);
}
showThis(); // In non-strict mode: window (browser) / global (Node.js)
            // In strict mode: undefined

'use strict';
function showThisStrict() {
    console.log(this);
}
showThisStrict(); // undefined
```

### Arrow Functions

Arrow functions do **not** have their own `this` binding. Instead, they capture (`this` is lexically bound) the `this` value from their **enclosing lexical scope** (the scope in which they were defined). This binding is fixed and cannot be changed by `call`, `apply`, or `bind`.

> **NOTE:** This characteristic makes them ideal for callbacks or methods where you want `this` to consistently refer to the surrounding object, avoiding common `this` context issues seen with regular functions in callbacks.

```js
const user = {
    firstName: "John",
    sayHi: function() {
        // Regular function: 'this' here is 'user'
        const arrowFunc = () => {
            // Arrow function: 'this' is lexically inherited from 'sayHi', so it's 'user'
            console.log(`Hello, ${this.firstName}`);
        };
        arrowFunc();
    },
    sayHiArrow: () => {
        // Arrow function: 'this' is lexically inherited from the global scope (window/global)
        console.log(`Hello, ${this.firstName}`); // 'this' is window/global, firstName is undefined
    }
};

user.sayHi();      // Hello, John
user.sayHiArrow(); // Hello, undefined (or error if strict mode and no global firstName)
```

### Event Handlers

In most DOM event handlers, `this` refers to the **element that received the event**.

```js
// <button onclick="console.log(this)">Click Me</button>
// 'this' inside the onclick handler refers to the button element.
```

### Constructor Functions

When a function is used as a constructor with the `new` keyword, `this` inside the constructor refers to the **newly created instance** of the object.

```js
function Person(name) {
    this.name = name; // 'this' refers to the new Person instance
}
const person1 = new Person("Alice");
console.log(person1.name); // Alice
```

---
## Use of `call`, `apply`, and `bind`

These three methods are part of `Function.prototype` and are used to explicitly control the `this` context of a function, and in the case of `call` and `apply`, to immediately invoke the function.

### `function.call(thisArg, arg1, arg2, ...)`

* Immediately invokes the function.
* Allows you to explicitly set the `this` context (`thisArg`) for the function call.
* Accepts arguments individually, passed after the `thisArg`.
* Useful when you know the exact number of arguments and want to pass them directly.

```js
const person = { name: "Alice" };
function greet(greeting, punctuation) {
    console.log(`${greeting}, ${this.name}${punctuation}`);
}

greet.call(person, "Hello", "!"); // Sets 'this' to 'person', passes "Hello", "!" as args
                                  // Output: "Hello, Alice!"
```

### `function.apply(thisArg, [argsArray])`

* Immediately invokes the function.
* Allows you to explicitly set the `this` context (`thisArg`) for the function call.
* Accepts arguments as an **array (or array-like object)**. This is the key difference from `call`.
* Useful when you have an array of arguments or when the number of arguments is dynamic.

```js
const person = { name: "Bob" };
function greet(greeting, punctuation) {
    console.log(`${greeting}, ${this.name}${punctuation}`);
}

const args = ["Hi", "..."];
greet.apply(person, args); // Sets 'this' to 'person', passes args from array
                           // Output: "Hi, Bob..."
```

### `function.bind(thisArg, arg1, arg2, ...)`

* **Does not immediately invoke** the function.
* Returns a **new function** (a "bound function") with its `this` context permanently bound to `thisArg`. Any arguments passed to `bind` after `thisArg` are "pre-filled" (curried) as leading arguments to the new function.
* The `this` binding of a function created with `bind` cannot be overridden later, even with `call` or `apply`.

> **NOTE:** Extremely useful for creating functions that need a specific `this` context for later execution, such as event handlers, callbacks, or when passing methods to other functions.

```js
const person = {
    name: "Charlie",
    greet: function(greeting) {
        console.log(`${greeting}, ${this.name}`);
    }
};

const boundGreet = person.greet.bind(person, "Hey"); // Creates a new function
boundGreet(); // Invokes the new function, 'this' is 'person', 'greeting' is "Hey"
              // Output: "Hey, Charlie"

// Example with setTimeout where 'this' context is lost without bind
const user = {
    name: "David",
    logName: function() {
        console.log(this.name);
    }
};
setTimeout(user.logName, 100); // Logs undefined (or global name) because logName is called as a standalone function
setTimeout(user.logName.bind(user), 100); // Logs "David" (bind ensures 'this' is 'user')
```

---

#javascript