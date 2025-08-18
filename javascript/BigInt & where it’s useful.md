# `BigInt` & where it’s useful

**`BigInt`** is a new primitive data type introduced in ES11 (ECMAScript 2020) that allows JavaScript to represent and perform operations on integers larger than `2^53 - 1` (the maximum safe integer that can be represented by the standard `Number` type).

---
## Creating a BigInt

You can create a BigInt by appending n to the end of an integer literal or by calling the BigInt() constructor.

```js
const bigIntLiteral = 9007199254740991n; // Add 'n' suffix
const bigIntFromNumber = BigInt(9007199254740991); // Convert a Number
const bigIntFromString = BigInt("9007199254740992345"); // From a string
```

---
## Key Characteristics

* **Arbitrary Precision**: `BigInt` can represent integers of arbitrary size, limited only by available memory.
* **Separate Type**: `BigInt` is a distinct primitive type. You cannot mix `BigInt` with regular `Number` values in most arithmetic operations without explicit conversion, which will result in a `TypeError`.
* **Operations**: Supports standard arithmetic operations (`+`, `-`, `*`, `/`, `%`, `**`), bitwise operations, and comparisons. Division (`/`) with `BigInt` truncates any fractional part (it does not return a floating-point number).

```js
const largeNum = 1000000000000000000n;
const anotherLargeNum = 2n;
console.log(largeNum * anotherLargeNum); // 2000000000000000000n
console.log(largeNum / 3n); // 333333333333333333n (truncated)

// console.log(largeNum + 1); // TypeError: Cannot mix BigInt and other types
console.log(largeNum + BigInt(1)); // 1000000000000000001n
```

---
## Where it's useful

* **Large Integer IDs**: When dealing with very large database IDs or unique identifiers that exceed `Number.MAX_SAFE_INTEGER`.
* **Financial Calculations**: Although typically handled by dedicated decimal libraries, `BigInt` can be used for integer-based financial calculations where precision is paramount and floating-point errors are unacceptable.
* **Cryptographic Operations**: Many cryptographic algorithms involve operations on extremely large integers.
* **Working with APIs**: When consuming APIs that return large integer values (e.g., Twitter API's `id_str`).

> **IMPORTANT**: `BigInt` fills a critical gap in JavaScript's numeric capabilities, allowing it to handle large integer values precisely without resorting to string-based workarounds or external libraries.

---

#javascript