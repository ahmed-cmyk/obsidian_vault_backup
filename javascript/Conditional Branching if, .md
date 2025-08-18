# Conditional Branching: `if`, `?`

We often need to execute different code depending on a condition.

JavaScript provides:

* `if … else if … else`
* The **conditional (ternary) operator** `?`

---

## The `if` Statement

Evaluates a condition and, if `true`, executes the block:

```js
if (year == 2015) {
  alert("That's correct!");
  alert("You're so smart!");
}
```

> **Recommendation:** Always use curly braces `{ }` for readability, even with a single statement.

---

## Boolean Conversion

The condition inside `if (…)` is converted to a boolean:

* Falsy: `0`, `""`, `null`, `undefined`, `NaN`
* Truthy: everything else

Examples:

```js
if (0) { /* never runs */ }
if (1) { /* always runs */ }
```

You can also pre-compute the boolean:

```js
let cond = (year == 2015);
if (cond) { … }
```

---

## The `else` Clause

Executes when the condition is falsy:

```js
if (year == 2015) {
  alert("You guessed it right!");
} else {
  alert("How can you be so wrong?");
}
```

---

## Multiple Conditions: `else if`

Chains conditions:

```js
if (year < 2015) {
  alert("Too early…");
} else if (year > 2015) {
  alert("Too late");
} else {
  alert("Exactly!");
}
```

---

## The Conditional (Ternary) Operator `?`

Shorter syntax for returning one of two values:

```js
let result = condition ? value1 : value2;
```

Example:

```js
let accessAllowed = (age > 18) ? true : false;
```

> **Note:** In this case `age > 18` already evaluates to `true/false`, so you can write:

```js
let accessAllowed = age > 18;
```

---

## Multiple `?`

You can chain `?` to handle more than two cases:

```js
let message =
  (age < 3) ? 'Hi, baby!' :
  (age < 18) ? 'Hello!' :
  (age < 100) ? 'Greetings!' :
  'What an unusual age!';
```

Equivalent `if..else`:

```js
if (age < 3) {
  message = 'Hi, baby!';
} else if (age < 18) {
  message = 'Hello!';
} else if (age < 100) {
  message = 'Greetings!';
} else {
  message = 'What an unusual age!';
}
```

---

## Non-Traditional Use of `?`

You *can* use `?` just to execute one of two expressions:

```js
(company == 'Netscape') ?
  alert('Right!') : alert('Wrong.');
```

But this is less readable than `if..else`:

```js
if (company == 'Netscape') {
  alert('Right!');
} else {
  alert('Wrong.');
}
```

> **Recommendation:** Use `?` only when you want to return one value or another. Use `if` when executing code blocks.

---

#javascript