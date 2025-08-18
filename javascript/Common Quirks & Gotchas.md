# Common Quirks & Gotchas

JavaScript, despite its widespread use, has a few unique behaviors and "gotchas" that can surprise developers, especially those new to the language. Understanding these quirks is key to writing robust and predictable code.

## Why is `NaN !== NaN`?

`NaN` stands for "Not-a-Number" and is a special numeric value that represents an undefined or unrepresentable numerical result (e.g., `0 / 0`, `Math.sqrt(-1)`, `parseInt('hello')`).

> **NOTE:** The most peculiar characteristic of `NaN` is that it is the **only value in JavaScript that is not equal to itself**, including when compared using strict equality (`===`) or loose equality (`==`).

```js
console.log(NaN === NaN); // false
console.log(NaN == NaN);   // false
```

> **ADV:** This behavior is specified in the IEEE 754 standard for floating-point arithmetic, which JavaScript adheres to for its number type. The rationale is that `NaN` represents an invalid or unrepresentable result, and any two invalid results are not necessarily "the same" invalid result.

### How to check for NaN?

Because NaN !== NaN, you cannot use equality operators to check if a value is NaN. Instead, use:

* **`isNaN()`global function**: This function checks if a value is `NaN` OR if it *coerces to `NaN`*. This means `isNaN('hello')` is `true` because `'hello'` converts to `NaN` when attempting a numeric operation.
* **`Number.isNaN()`(preferred)**: Introduced in ES6, this method is more robust. It checks if a value is *strictly* `NaN` and does not perform type coercion. It's the most reliable way to check for `NaN`.

```js
console.log(isNaN(NaN));          // true
console.log(isNaN('hello'));      // true (because 'hello' coerces to NaN)

console.log(Number.isNaN(NaN));   // true
console.log(Number.isNaN('hello')); // false (no coercion)
```

---
## Why is `typeof null` equal to `"object"`?

This is a well-known, long-standing bug or "quirk" in JavaScript that dates back to the very first version of the language. It's a historical accident that has never been fixed to avoid breaking existing web code.

Conceptually, `null` represents the intentional absence of any object value. However, `typeof null` returns `"object"`.

```js
console.log(typeof null); // "object"
```

The prevailing explanation is that in the initial implementation of JavaScript, values were represented internally by a type tag and a value. For objects, the type tag was `0`. `null` was represented as `0` (the null pointer), and thus its type tag was `0`, leading `typeof` to incorrectly report `"object"`.

### How to check for null?

Since typeof null is unreliable for distinguishing null from actual objects, the correct way to check if a value is strictly null is to use the strict equality operator:

```js
const value = null;
console.log(value === null); // true
```

---
## What does `[] + []`, `[] + {}`, and `{}` vs `({})` produce?

These examples highlight JavaScript's implicit type coercion rules, particularly when the `+` operator is used in non-numeric contexts, and the ambiguity of curly braces.

* **`[] + []`**:

  * Result: `""` (empty string)
  * Explanation: When the `+` operator is used with operands that are not numbers and at least one is an object (like an array), JavaScript attempts to convert them to primitive values. For arrays, `[].toString()` results in an empty string `""`. So, `"" + ""` results in `""`.

```js
console.log([] + []); // ""
```

* **`[] + {}`**:

  * Result: `"[object Object]"`
  * Explanation: Again, `[]` converts to `""`. For `{}` (a plain object), `{}.toString()` results in the string `"[object Object]"`. So, `"" + "[object Object]"` results in `"[object Object]"`.

```js
console.log([] + {}); // "[object Object]"
```

* **`{} + []`**:

  * Result: `0` (in browser console/Node.js REPL) OR `"[object Object]"` (in a script file)
  * Explanation: This is a tricky one due to **ambiguity in parsing**.
    * When `{}` appears at the **beginning of a statement** (like in a console or a script file where it's the first token on a line), JavaScript parses it as an **empty block statement**, not an empty object literal. The block statement is ignored. Then, `+ []` is evaluated. The unary `+` operator attempts to convert `[]` to a number. `[].toString()` is `""`, and `Number("")` is `0`.
    * When `{}` is **not at the beginning of a statement** (e.g., wrapped in parentheses `({})` or part of an expression), it's parsed as an **object literal**. In this case, `({} + [])` would result in `"[object Object]"`.

```js
// In browser console or Node.js REPL:
// {} + [] // 0

// In a script file:
// {} + [] // 0

// If forced to be an expression:
console.log({} + []); // "[object Object]" (in a script, this is usually wrapped in console.log)
console.log({} + 1); // "[object Object]1"
```

* **`{}` vs `({})`**:

  * `{}`: When used as a standalone statement, it's parsed as an **empty block statement**.
  * `({})`: The parentheses force the curly braces to be parsed as an **empty object literal**.

```js
// In a script:
// {} // This is an empty block, does nothing
// console.log({}); // This logs an empty object because it's part of an expression

console.log({}); // Logs {} (an empty object)
console.log(({})); // Logs {} (an empty object)
```

These examples highlight JavaScript's flexible but sometimes confusing type coercion rules and parsing ambiguities.

---
## Why `0.1 + 0.2 !== 0.3` (floating-point precision)

> **IMPORTANT:** This is not a JavaScript-specific quirk but a fundamental characteristic of how **floating-point numbers** are represented and calculated in virtually all programming languages that use the IEEE 754 standard (which JavaScript does for its `number` type).

Computers store numbers in binary. Many decimal fractions, like `0.1`, `0.2`, and `0.3`, cannot be represented precisely as finite binary fractions. They become repeating binary fractions, similar to how 1/3 is a repeating decimal (0.333...). When these repeating binary fractions are truncated to fit into the fixed-size memory allocated for floating-point numbers, tiny precision errors are introduced.

```js
console.log(0.1 + 0.2); // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3); // false
```

The sum of `0.1` and `0.2` results in a number that is infinitesimally larger than `0.3` due to these rounding errors, causing the strict equality check to fail.

### How to handle floating-point comparisons

* **Use a small epsilon value**: Compare if the absolute difference between two numbers is less than a very small threshold.

```js
function areEqual(a, b, epsilon = 0.0000001) {
    return Math.abs(a - b) < epsilon;
}
console.log(areEqual(0.1 + 0.2, 0.3)); // true
```

* **Work with integers**: If possible, scale your numbers to integers (e.g., work with cents instead of dollars) to perform calculations, then convert back.
* **Use a dedicated math library**: For financial or scientific calculations requiring high precision, use libraries specifically designed for arbitrary-precision arithmetic (e.g., `Big.js`, `Decimal.js`).

---
## Automatic Semicolon Insertion (ASI)

**Automatic Semicolon Insertion (ASI)** is a mechanism in JavaScript where the engine automatically inserts semicolons into the code at certain points during parsing, even if they are omitted by the developer. This is designed to make JavaScript more forgiving, but it can also lead to unexpected behavior if not understood.

**ASI rules are complex, but generally, a semicolon is inserted when:**

* A newline character is encountered, and the next token cannot be parsed as a continuation of the current statement.
* A `}` (closing curly brace) is encountered.
* The end of the program input is reached.
* A `return`, `throw`, `break`, `continue`, `yield`, or `async` keyword is followed by a newline.

### Common Gotchas with ASI

* **`return`statement**: If you put the return value on a new line, ASI will insert a semicolon after `return`, causing the function to return `undefined`.

```
function getNumber() {
    return // ASI inserts semicolon here
    10;
}
console.log(getNumber()); // undefined
```

* **IIFEs and immediately invoked functions**: If the previous line doesn't end with a semicolon, and the current line starts with `(`, it might be interpreted as a function call.

```
let x = 5
(function() { console.log('IIFE'); })(); // This might cause an error if 'x' isn't terminated
// Interpreted as: let x = 5(function() { ... })()
```

### Best Practice

While ASI exists, it is widely recommended to always explicitly terminate your statements with semicolons (;). This makes your code more predictable, less prone to subtle bugs, and easier for linters and other tools to analyze.

---
## Why you should avoid `with` statement

The `with` statement is a deprecated feature in JavaScript that extends the scope chain for a statement or block. It takes an object and adds all its properties to the scope chain of the code block.

```
const user = {
    firstName: "John",
    lastName: "Doe"
};

// DON'T DO THIS!
with (user) {
    console.log(firstName + " " + lastName); // Accesses properties directly from 'user'
}
```

### Reasons to Avoid `with`

* **Performance Issues**: It makes scope resolution much slower because the JavaScript engine cannot determine at compile time where a variable is coming from (is it a local variable or a property of the `with` object?). This prevents many optimizations.
* **Readability and Maintainability**: It makes code harder to read and understand. It's unclear whether a variable refers to a property of the `with` object or a variable in an outer scope. This ambiguity leads to "silent" bugs.
* **Strict Mode Restriction**: The `with` statement is **forbidden in strict mode**. This alone is a strong reason to avoid it, as strict mode is a best practice for modern JavaScript development.
* **Security Concerns**: It can potentially introduce security vulnerabilities by making it easier to accidentally modify properties on objects you didn't intend to.

Modern JavaScript provides better alternatives like **destructuring assignment** for concisely accessing object properties without the downsides of `with`.

```js
// Preferred alternative using destructuring
const { firstName, lastName } = user;
console.log(firstName + " " + lastName);
```

> **IMPORTANT:** The `with` statement is considered a legacy feature and should never be used in new code.

---
#javascript