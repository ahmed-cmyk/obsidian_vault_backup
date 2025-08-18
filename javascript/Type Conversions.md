# Type Conversions

JavaScript automatically converts values to the expected type in most operations.

**For example:**

* `alert()` converts values to strings.
* Math operations convert values to numbers.

Sometimes, explicit conversion is needed.

> **Note:** These notes only cover **primitives** — objects are handled later in *Object to primitive conversion*.

---

## String Conversion

String conversion occurs when a string representation is required.
For example, `alert(value)` implicitly converts its argument to a string.

Explicit conversion is done using `String(value)`:

```js
let value = true;
alert(typeof value); // boolean
value = String(value);
alert(typeof value); // string
```

**The conversion is intuitive:**

* `false` → `"false"`
* `null` → `"null"`

---

## Numeric Conversion

Happens automatically in mathematical operations and comparisons.

**For example:**

```js
alert("6" / "2"); // 3
```

Explicit conversion is done using `Number(value)`:

```js
let str = "123";
alert(typeof str); // string

let num = Number(str);
alert(typeof num); // number
```

If the string cannot be converted to a valid number, the result is `NaN`.

### Rules

| Value | Converts to |
|---|---|
| `undefined` | `NaN` | 
| `null` | `0` | 
| `true` / `false` | `1` / `0` | 
| string | trimmed & parsed — empty → `0`, invalid → `NaN` | 

### Examples

```js
alert(Number("   123   "));   // 123
alert(Number("123z"));        // NaN
alert(Number(true));          // 1
alert(Number(false));         // 0
```

> **Important:** `null` becomes `0`, but `undefined` becomes `NaN`.

---

## Boolean Conversion

Boolean conversion happens in logical contexts (`if`, `while`, etc.) or explicitly via `Boolean(value)`.

Values considered “empty” become `false`:
`0`, `null`, `undefined`, `NaN`, `""`

All other values become `true`.

### Examples

```
alert(Boolean(1));           // true
alert(Boolean(0));           // false

alert(Boolean("hello"));     // true
alert(Boolean(""));          // false
```

> **Caution:** Strings `"0"` and `" "` are **truthy**, because any non-empty string is considered `true`.

```js
alert(Boolean("0"));         // true
alert(Boolean(" "));         // true
```

---

## Summary Table

### String Conversion

* Happens automatically when outputting.
* Can be done explicitly with `String(value)`.
* Intuitive: primitive values become their string representation.

### Numeric Conversion

* Happens in mathematical operations.
* Can be done explicitly with `Number(value)`.
* Rules:

| Value | Converts to |
|---|---|
| `undefined` | `NaN` | 
| `null` | `0` | 
| `true` / `false` | `1` / `0` | 
| string | trimmed, empty → `0`, invalid → `NaN` | 

### Boolean Conversion

* Happens in logical contexts.
* Can be done explicitly with `Boolean(value)`.
* Rules:

| Value | Converts to |
|---|---|
| `0`, `null`, `undefined`, `NaN`, `""` | `false` | 
| anything else | `true` | 

---

**Caution:**

* `undefined → NaN` as a number, not `0`.
* `"0"` and `" "` are truthy, even though they *look empty*.

---

If you’d like, I can also prepare a concise cheat-sheet or a set of tricky interview questions based on these conversions. Let me know.#javascript