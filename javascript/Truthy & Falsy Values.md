# Truthy & Falsy Values

## Falsy values:

* `false`
* `0`
* `-0`
* `""`
* `null`
* `undefined`
* `NaN`

Example:

```js
if (0) { /* does not run */ }
```

---
## Truthy values:

* Everything else, including:
  * Non-zero numbers
  * Non-empty strings
  * All objects
  * Functions, `Symbol`, `BigInt` (except `0n`)

**Example:**

```js
if ([]) {
  console.log("Empty array is truthy!");
}
```

---

#javascript