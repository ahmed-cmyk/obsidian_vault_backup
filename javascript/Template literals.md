# Template literals

**Template literals** (also known as template strings) are a powerful ES6 feature that provide an enhanced way to create strings in JavaScript. They are enclosed by backticks (`\``) instead of single or double quotes.

---
## Key Features

### Multi-line Strings

Template literals can span multiple lines without needing special escape characters (`\n`). This makes writing long strings or embedding HTML much cleaner.

```
const multiLineString = `
This is a string
that spans
multiple lines.
`;
console.log(multiLineString);
```

### String Interpolation

They allow embedded expressions within the string using the `${expression}` syntax. The expression's result is converted to a string and inserted into the template. This eliminates the need for string concatenation (`+`).

```js
const name = "Alice";
const age = 30;
const greeting = `Hello, my name is ${name} and I am ${age} years old.`;
console.log(greeting); // Hello, my name is Alice and I am 30 years old.

const sum = 5 + 3;
console.log(`The sum is ${sum}.`); // The sum is 8.
```

### Tagged Templates

A more advanced feature where a function can "tag" a template literal. The function receives the string parts and the evaluated expressions as arguments, allowing for custom parsing and manipulation of the template literal. 

> **NOTE**: This is used for advanced use cases like internationalization, escaping HTML, or creating domain-specific languages.

```js
function highlight(strings, ...values) {
    let str = '';
    strings.forEach((string, i) => {
        str += string;
        if (values[i]) {
            str += `<b>${values[i]}</b>`;
        }
    });
    return str;
}

const name = "Alice";
const age = 30;
const message = highlight`My name is ${name} and I am ${age} years old.`;
console.log(message); // My name is <b>Alice</b> and I am <b>30</b> years old.
```

> **NOTE**: Template literals significantly improve the readability and flexibility of string manipulation in modern JavaScript.

---

#javascript