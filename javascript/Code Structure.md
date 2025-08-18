# Code Structure

The building blocks of JS code are as follows.

## Statements

Statements are syntax constructs and commands that perform actions. We can have as many statements in our code as we want. Statements can be separated with a semicolon.

**Example:**

```js
alert('Hello');
```

## Semicolons

**Semicolons** are used to indicate that two statements are separate. 

JavaScript interprets the line break as an “implicit” semicolon. This is called an [automatic semicolon insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion).

**Example:**

```js
alert('Hello')
alert('World')
```

There are cases when a newline does not mean a semicolon. For example:

```js
alert(3 +
1
+ 2);
```

> **NOTE:** But there are situations where JavaScript “fails” to assume a semicolon where it is really needed.

## Comments

Comments are used to describe the code and explain why it exists and what it’s used for.

### Single Line Comments

```js
// This comment occupies a line of its own
alert('Hello');
alert('World'); // This comment follows the statement
```

### Multiline Comments

```js
/* An example with two messages.
This is a multiline comment.
*/
alert('Hello');
alert('World');
```

---

#javascript