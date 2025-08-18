# Arrays

## Array Destructuring:

> **NOTE**: Destructuring is an ES6 feature.

You can extract elements from an array by matching their position.

```js
const colors = ["red", "green", "blue"];

// Basic destructuring
const [firstColor, secondColor] = colors;
console.log(firstColor);  // "red"
console.log(secondColor); // "green"

// Skipping elements
const [,, thirdColor] = colors;
console.log(thirdColor); // "blue"

// Rest parameter with destructuring
const [primary, ...restColors] = colors;
console.log(primary);     // "red"
console.log(restColors);  // ["green", "blue"]

// Default values for missing elements
const [c1, c2, c3, c4 = "purple"] = colors;
console.log(c4); // "purple"
```

---
#javascript