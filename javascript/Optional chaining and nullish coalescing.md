# Optional chaining and nullish coalescing

ES2020 introduced two powerful operators that simplify working with potentially `null` or `undefined` values, reducing the need for verbose conditional checks.

---
## Optional Chaining (?.)

The optional chaining operator (?.) allows you to safely access properties or call methods on an object that might be null or undefined without causing a TypeError. If the part before ?. is null or undefined, the expression short-circuits and evaluates to undefined.

### Accessing properties

```js
const user = {
    name: "Alice",
    address: {
        street: "123 Main St",
        zip: "10001"
    }
};

console.log(user.address?.street);    // "123 Main St"
console.log(user.contact?.email);     // undefined (no error)

const admin = null;
console.log(admin?.name);             // undefined (no error)
```

### Calling methods

```js
const userProfile = {
    getName() { return "Alice"; }
};
const guestProfile = null;

console.log(userProfile.getName?.());  // "Alice"
console.log(guestProfile?.getName?.()); // undefined (no error)
```

### Accessing array elements

```js
const users = [{ id: 1 }];
console.log(users?.[0]?.id); // 1
console.log(users?.[1]?.id); // undefined
```

Optional chaining is a concise and safe way to navigate deeply nested object structures.

---
## Nullish Coalescing Operator (??):

The nullish coalescing operator (??) provides a way to define a default value for a variable that is null or undefined. It returns its right-hand side operand when its left-hand side operand is null or undefined; otherwise, it returns its left-hand side operand.

This differs from the logical OR operator (`||`), which returns the right-hand side if the left-hand side is any falsy value (including `0`, `''`, `false`). `??` specifically checks for `null` or `undefined`.

```js
const userInput = null;
const defaultName = "Guest";

// Using ??
const name = userInput ?? defaultName;
console.log(name); // "Guest"

const count = 0;
const defaultCount = 1;
const actualCount = count ?? defaultCount;
console.log(actualCount); // 0 (because 0 is not null or undefined)

// Compare with ||
const value = 0;
const fallback = 10;
const resultWithOR = value || fallback;
console.log(resultWithOR); // 10 (because 0 is falsy)

const resultWithNullish = value ?? fallback;
console.log(resultWithNullish); // 0 (because 0 is not null/undefined)
```

> **NOTE**: The nullish coalescing operator is ideal when you want to provide a fallback value only when a variable is explicitly `null` or `undefined`, allowing `0`, `''`, or `false` to be considered valid values.

---

#javascript