# Assertion Errors

An `AssertionError` in Node.js is an error that is thrown when the `assert` module determines that a given expression is not truthy. The `assert` module is a built-in Node.js module that provides a simple set of assertion tests that can be used to test the behavior of your code.

---
## AssertionError in Node.js

### What is it?

An `AssertionError` is a specific kind of **error object** that is thrown by Node.js’s built‑in `assert` module when an assertion fails.
The `assert` module is typically used for writing tests, sanity checks, and debugging logic in code.
It helps ensure that certain conditions in your program hold true — if not, it interrupts the execution with an `AssertionError`.

---

### Where does it come from?

It comes from the [`assert`](https://nodejs.org/api/assert.html) module:

```
const assert = require('assert');

assert(1 === 1);  // ✅ No error
assert(1 === 2);  // ❌ Throws AssertionError
```

When the given condition is falsy, Node throws an `AssertionError` object.

---

### Anatomy of an AssertionError

When thrown, the error has several useful properties:

* `name`: always `"AssertionError"`
* `actual`: the actual value received
* `expected`: the expected value
* `operator`: the operator used in the test (`===`, `!==`, etc.)
* `message`: an optional error message
* `generatedMessage`: whether the message was auto‑generated

Example:

```js
const assert = require('assert');

assert.strictEqual(1, 2, 'Values are not equal');
```

Output:

```js
AssertionError [ERR_ASSERTION]: Values are not equal
  actual: 1
  expected: 2
  operator: 'strictEqual'
```

---

### Common assertion methods

The `assert` module provides several assertion methods:

* `assert(value[, message])`
* `assert.strictEqual(actual, expected[, message])`
* `assert.notStrictEqual(actual, expected[, message])`
* `assert.deepStrictEqual(actual, expected[, message])`
* `assert.throws(fn[, error][, message])`
* `assert.doesNotThrow(fn[, error][, message])`
* `assert.ok(value[, message])` — same as `assert(value)`

---

### Example: assert.throws

You can also assert that a function throws an error:

```js
assert.throws(
  () => {
    throw new Error('fail');
  },
  Error
);
```

If the function does not throw an `Error`, an `AssertionError` is raised instead.

---

### When to use?

* Unit testing (assert expected outcomes)
* Checking assumptions during development/debugging
* Validating invariants in internal logic

---

### Notes

> **Important:** `assert` is **synchronous** and should not be used for validating user input or production‑critical logic — it’s best for testing and debugging only.
> In production, a failing assertion terminates the process unless caught, which may not be desirable.

---

#nodejs