# Scope Chain & Closures

The **scope chain** dictates how variables are resolved in JavaScript.

When a variable is referenced:

* The engine looks for it in the current scope.
* If not found, it moves up to the immediate outer (parent) scope.
* This continues upward through all enclosing scopes until it reaches the global scope.

This chain of scopes is determined **lexically** — by where functions are *written* in the code, not where they are *called*.

---

## Closures

A **closure** is formed when a function "remembers" its lexical (outer) environment even after the outer function has finished executing.

This enables the inner function to access variables, parameters, and functions from its outer scope even after that outer scope is gone.

When an inner function is returned or passed elsewhere, it carries with it a reference to its creation environment.

---

### Example

```js
function createAdder(x) {
    // 'x' is part of the outer lexical environment
    return function (y) {
        return x + y;
    };
}

const addFive = createAdder(5); 
console.log(addFive(10)); // 15

const addTen = createAdder(10); 
console.log(addTen(20)); // 30
```

In this example:

* `addFive` is a closure that "remembers" `x = 5`.
* `addTen` is a separate closure that "remembers" `x = 10`.

---

## Useful for

* Data encapsulation
* Maintaining private state
* Implementing patterns like currying and the module pattern

---

#javascript