# Data Types

## Basics
Every value in JavaScript has a type. JavaScript is **dynamically typed**, meaning variables are not bound to a fixed type and can hold different types at runtime.

```js
let message = "hello";
message = 123; // valid
```

## number
The number type represents both integers and floating-point numbers.
It includes three special values:
* Infinity (greater than any number, e.g., 1 / 0)
* -Infinity (less than any number)
* NaN (result of invalid math, like "abc" / 2)

> **NOTE:** NaN is *sticky*: any calculation involving it results in NaN, except for the edge case NaN ** 0 === 1.

Mathematical operations are “safe” in JavaScript — invalid operations don’t crash your code but instead return NaN or Infinity.

## bigint
The bigint type allows you to work with integers larger than ±(2^53 - 1) (9007199254740991, also known as Number.MAX_SAFE_INTEGER).
Beyond that, number loses precision. You create a bigint by appending n to an integer:

```js
const bigInt = 12345678901234567890n;
```

bigint is rarely used, mostly for cryptography or very high-precision timestamps.

## string
The string type represents text, which must be enclosed in quotes. JavaScript supports three kinds of quotes:
* Double quotes: "text"
* Single quotes: 'text'
* Backticks: `text`

⠀Backticks allow embedding of variables and expressions:

```js
let name = "John";
`Hello, ${name}`
```

There is no char type in JavaScript — a single character is simply a string of length 1.

## boolean
The boolean type has only two values: true and false. It is often the result of a comparison:
### let isGreater = 4 > 1; // true

## null
The null type has a single value: null. It represents “nothing”, “empty”, or “unknown”. Unlike in some languages, it is not a pointer or reference.

## undefined
The undefined type also has only one value: undefined. A declared but uninitialized variable automatically gets this value.
Although you *can* assign undefined explicitly, it’s discouraged — prefer null to represent intentional “emptiness”.

## symbol
The symbol type creates unique identifiers, often used as object keys. It is rarely needed in day-to-day programming.

# object
The object type is used for collections and more complex entities. Unlike primitives, which hold single values, objects can contain properties and methods.
Even Math is an object.

## typeof operator
The typeof operator returns the type of its operand as a string. It can be written as typeof x or typeof(x) — both are equivalent.

**Examples:**

```js
typeof undefined           // "undefined"
typeof 0                   // "number"
typeof 10n                 // "bigint"
typeof true                // "boolean"
typeof "foo"               // "string"
typeof Symbol("id")        // "symbol"
typeof Math                // "object"
typeof null                // "object" (bug — null is *not* actually an object)
typeof alert               // "function" (functions are technically objects)
```

**Notes:**
* typeof null === "object" is a long-standing bug in JavaScript kept for compatibility.
* typeof alert === "function" is technically incorrect but convenient and preserved behavior.

## Summary Table

| **Type** | **Notes** |
|:-:|:-:|
| number | Integers & floats; includes NaN, Infinity, -Infinity |
| bigint | Arbitrary-precision integers |
| string | Text, no char type |
| boolean | true / false |
| null | Represents nothing, unknown |
| undefined | Uninitialized variable |
| symbol | Unique identifiers |
| object | Collections and complex entities |

---

#javascript