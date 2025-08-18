# Basic Operators & Math

JavaScript supports familiar arithmetic operators from school: addition `+`, subtraction `-`, multiplication `*`, division `/`, etc.

---

# Terms: Unary, Binary, Operand

An **operand** is what an operator acts on.
For example: in `5 * 2`, the operands are `5` and `2`.

An operator is **unary** if it takes one operand:

```
let x = 1;
x = -x;
alert(x); // -1
```

An operator is **binary** if it takes two operands:

```
let x = 1, y = 3;
alert(y - x); // 2
```

> **Note:** The same `-` symbol works as both unary negation and binary subtraction.

---

# Arithmetic Operations

JavaScript supports:

* Addition: `+`
* Subtraction: `-`
* Multiplication: `*`
* Division: `/`
* Remainder: `%`
* Exponentiation: `**`

---

## Remainder `%`

Returns the remainder of integer division:

```js
alert(5 % 2); // 1
alert(8 % 3); // 2
alert(8 % 4); // 0
```

---

## Exponentiation `**`

Raises a number to a power:

```js
alert(2 ** 3); // 8
alert(4 ** (1/2)); // 2 (square root)
alert(8 ** (1/3)); // 2 (cube root)
```

---

# String Concatenation with `+`

If any operand of `+` is a string, both are converted to strings and concatenated:

```js
alert('1' + 2);     // "12"
alert(2 + '1');     // "21"
alert(2 + 2 + '1'); // "41" (numbers first, then string)
alert('1' + 2 + 2); // "122" (string first → all strings)
```

> **Important:** Only `+` behaves this way. Other arithmetic operators always convert operands to numbers.

**Other examples:**

```js
alert(6 - '2');     // 4
alert('6' / '2');   // 3
```

---

# Unary Plus `+`

The unary `+` converts its operand to a number:

```js
alert(+true); // 1
alert(+"");   // 0
```

Equivalent to `Number(value)` but shorter.

**Example with form inputs:**

```js
let apples = "2";
let oranges = "3";

alert(apples + oranges);           // "23" (string concatenation)
alert(+apples + +oranges);         // 5   (converted to numbers)
```

> **Important:** Unary operators have higher precedence than binary ones.

---

# Operator Precedence

When multiple operators appear, precedence determines the order of evaluation.
Higher precedence → executes first. Parentheses override precedence.

Excerpt from the precedence table:

| Precedence | Operator |
|---|---|
| 14 | Unary `+`, `-` | 
| 13 | `**` | 
| 12 | `*`, `/` | 
| 11 | `+`, `-` (binary) | 
| 2 | `=` (assignment) | 

```js
let x = 2 * 2 + 1;  // Multiplication before addition
alert(x); // 5
```

---

# Assignment `=`

Assignment is also an operator and returns a value:

```js
let a = 1;
let b = 2;

let c = 3 - (a = b + 1);

alert(a); // 3
alert(c); // 0
```

---

## Chaining Assignments

Assignments can be chained, evaluated right-to-left:

```js
let a, b, c;
a = b = c = 2 + 2;

alert(a); // 4
alert(b); // 4
alert(c); // 4
```

> **Note:** For clarity, split chained assignments into separate lines.

---

# Modify-in-place

Short forms exist for operations combined with assignment:

```js
let n = 2;
n += 5;  // n = n + 5
n *= 2;  // n = n * 2
```

> **Note:** `+=`, `*=`, `-=`, `/=`, etc., have the same precedence as `=`.

---

# Increment / Decrement

These operators increase or decrease a variable by 1:

```js
let counter = 2;

counter++; // 3
counter--; // 1
```

### Prefix vs. Postfix

* Prefix (`++counter`): increments and returns the new value.
* Postfix (`counter++`): returns the old value before incrementing.

Examples:

```
let counter = 1;
alert(++counter); // 2
alert(counter++); // 1
```

> **Caution:** Increment/decrement works only on variables, not on values like `5++`.

---

# Bitwise Operators

Work on the binary representation of numbers:

* AND: `&`
* OR: `|`
* XOR: `^`
* NOT: `~`
* Left shift: `<<`
* Right shift: `>>`
* Zero-fill right shift: `>>>`

> **Note:** Rarely used in web development but useful in cryptography and low-level tasks.

---

# Comma Operator `,`

Evaluates multiple expressions, returning the last one:

```js
let a = (1 + 2, 3 + 4);
alert(a); // 7
```

> **Caution:** Comma has very low precedence — use parentheses.

**Example in a `for` loop:**

```js
for (a = 1, b = 3, c = a * b; a < 10; a++) {
  // ...
}
```

> **Note:** Useful but can reduce readability — use with care.

---

#javascript