# Comparisons

JavaScript supports familiar comparison operators from mathematics:

* Greater / less than: `a > b`, `a < b`
* Greater / less than or equal: `a >= b`, `a <= b`
* Equals: `a == b` (note: `=` is assignment, not comparison)
* Not equals: `a != b` (≠ in math)

---

# Boolean Results

All comparison operators return a boolean:

```js
alert(2 > 1);    // true
alert(2 == 1);   // false
alert(2 != 1);   // true
```

You can assign the result to a variable:

```js
let result = 5 > 4;
alert(result);   // true
```

---

# String Comparison

Strings are compared lexicographically (dictionary order), character by character:

```js
alert('Z' > 'A');          // true
alert('Glow' > 'Glee');    // true
alert('Bee' > 'Be');       // true
```

Algorithm:

* Compare the first characters of both strings
* If different → result determined
* If equal → move to next character
* If one string ends → shorter string is considered smaller

> **Important:** Comparison is case-sensitive because it follows Unicode order. Uppercase letters come before lowercase.

---

# Comparison of Different Types

When comparing different types, JavaScript converts both values to numbers:

```js
alert('2' > 1);       // true
alert('01' == 1);     // true
```

For booleans, `true → 1`, `false → 0`:

```js
alert(true == 1);     // true
alert(false == 0);    // true
```

---

# A Funny Consequence

Sometimes a value is truthy in one context but equal to a falsy value:

```js
let a = 0;
let b = "0";

alert(Boolean(a));    // false
alert(Boolean(b));    // true
alert(a == b);        // true
```

> **Important:** `==` converts both to numbers before comparing, but `Boolean()` follows different rules.

---

# Strict Equality

Regular `==` performs type conversion:

```js
alert(0 == false);    // true
alert('' == false);   // true
```

Strict equality `===` does **not** convert types:

```js
alert(0 === false);   // false
```

> **Note:** There’s also strict inequality `!==`.

---

# Comparison with `null` and `undefined`

Strict equality (`===`):

```js
alert(null === undefined); // false
```

Loose equality (`==`):

```js
alert(null == undefined);  // true
```

For `>`, `<`, `>=`, `<=`, they’re converted to numbers:

* `null → 0`

* `undefined → NaN`

Examples:

```js
alert(null > 0);     // false
alert(null == 0);    // false
alert(null >= 0);    // true
```

> **Important:** Equality `==` and comparisons `>=` behave differently with `null`.

For `undefined`:

```js
alert(undefined > 0);  // false
alert(undefined < 0);  // false
alert(undefined == 0); // false
```

> **Important:** `undefined` becomes `NaN` in numeric comparisons, which is never >, <, or = to a number.

---

# Avoid Problems

> **Recommendation:**
> Avoid using `==` unless you specifically want type coercion.
> Use `===` for predictable comparisons.
> Be cautious with comparisons involving `null` or `undefined`, and check them explicitly.

---

# Summary

* Comparison operators return booleans.
* Strings are compared lexicographically.
* Different types are converted to numbers before comparing (except with `===`).
* `null` and `undefined` are equal to each other with `==` but unequal to anything else.
* Be cautious when comparing `null`/`undefined` with numbers — check explicitly.

---

#javascript