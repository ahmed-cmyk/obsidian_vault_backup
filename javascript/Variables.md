# Variables

## Declaration Styles

We can declare multiple variables in one line:

```js
let user = 'John', age = 25, message = 'Hello';
```

However, for the sake of readability this approach is not recommended. Rather the following approach where each variable is declared on a separate line is preferable.

```js
let user = 'John';
let age = 25;
let message = 'Hello';
```

Some people also define multiple variables in this multiline style:

```js
let user = 'John',
  age = 25,
  message = 'Hello';
```

…Or even in the “comma-first” style:

```js
let user = 'John'
  , age = 25
  , message = 'Hello';
```

---
## Variable naming

**There are two limitations on variable names in JavaScript:**
1. The name must contain only letters, digits, or the symbols $ and _.
2. The first character must not be a digit.

> **NOTE:** For naming camelCase is normally used.

> **IMPORTANT:** When it comes to naming variables it is important to note that **CASE MATTERS** as both `apple` and `APPLE` are two different variables.

**Some good-to-follow rules are:**
* Use human-readable names like `userName` or `shoppingCart`.
* Stay away from abbreviations or short names like `a`, `b`, and `c`, unless you know what you’re doing.
* Make names maximally descriptive and concise. Examples of bad names are `data` and `value`. Such names say nothing. It’s only okay to use them if the context of the code makes it exceptionally obvious which data or value the variable is referencing.
* Agree on terms within your team and in your mind. If a site visitor is called a “user” then we should name related variables `currentUser` or `newUser` instead of `currentVisitor` or `newManInTown`.

---
## Differences between `var`, `let`, and `const`

These keywords are used to declare variables, but they differ significantly in their scoping rules, hoisting behavior, and reassignability.

### **`var`**:

* **Scope**: Function-scoped. If declared outside any function, it is global-scoped.
* **Hoisting**: `var` declarations are hoisted to the top of their function or global scope and are initialized with `undefined`. This means you can access a `var` variable before its declaration without an error, though its value will be `undefined`.
* **Reassignment/Redeclaration**: Can be both reassigned and redeclared within the same scope without causing an error. This can sometimes lead to unexpected behavior or bugs.

```js
var x = 10;
var x = 20; // Valid, x is now 20
console.log(x); // 20
```

### **`let`**:

* **Scope**: Block-scoped. Variables are accessible only within the block (`{}`) where they are declared.
* **Hoisting**: `let` declarations are hoisted to the top of their block but are *not initialized*. They reside in a **Temporal Dead Zone (TDZ)** until their declaration is encountered during execution. Attempting to access them before this point results in a `ReferenceError`.
* **Reassignment/Redeclaration**: Can be reassigned but *cannot be redeclared* within the same scope.

```js
let y = 10;
y = 20; // Valid, y is now 20
// let y = 30; // SyntaxError: 'y' has already been declared
```

### **`const`**:

* **Scope**: Block-scoped, identical to `let`.
* **Hoisting**: Hoisted to the top of their block and also reside in the TDZ until declared. Accessing before declaration results in a `ReferenceError`.
* **Reassignment/Redeclaration**: Must be initialized at the time of declaration and **cannot be reassigned or redeclared**. For objects and arrays declared with `const`, the *binding* to the object/array cannot change, but the *contents* of the object or array itself can still be mutated.

```js
const z = 10;
// z = 20; // TypeError: Assignment to constant variable.
// const z = 30; // SyntaxError: 'z' has already been declared

const arr = [1, 2];
arr.push(3); // Valid, array content is mutable
console.log(arr); // [1, 2, 3]
```

---

#javascript