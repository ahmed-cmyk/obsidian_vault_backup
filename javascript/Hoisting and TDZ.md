# Hoisting and TDZ

**Hoisting** is a JavaScript mechanism where variable and function declarations are conceptually moved to the top of their containing scope during the compilation phase, *before* the code is executed.
Only the *declarations* are hoisted — not the initializations.

---

## `var` Hoisting

`var` declarations are hoisted and automatically initialized to `undefined`.
You can reference a `var` variable before its declaration in the code, but its value will be `undefined` at that point.

```js
console.log(myVar); // undefined
var myVar = 5;
console.log(myVar); // 5
```

---

## `let` and `const` Hoisting & TDZ

`let` and `const` declarations are also hoisted, but they are *not initialized*.

They enter a **Temporal Dead Zone (TDZ)** from the start of their block scope until their declaration line is executed.

Accessing them in the TDZ throws a `ReferenceError`.

> **NOTE:** This makes `let` and `const` behave more predictably and prevents bugs from accessing uninitialized variables.

```js
// console.log(myLet); // ReferenceError: Cannot access 'myLet' before initialization
let myLet = 10;
console.log(myLet); // 10
```

---

## Function Declaration Hoisting

Function declarations are fully hoisted:
Both their **name** and their **definition** move to the top of their scope.
You can call a function *anywhere* in its scope, even before the code where it’s written.

```js
myFunction(); // "Hello from function!"

function myFunction() {
    console.log("Hello from function!");
}
```

---

#javascript