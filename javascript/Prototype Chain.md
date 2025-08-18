# Prototype Chain

The **prototype chain** is a fundamental mechanism in JavaScript that enables inheritance.  

Every JavaScript object has an internal property `[[Prototype]]` (exposed as `__proto__` in many environments) which points to another object — its prototype.  

**When you try to access a property or method:**

- If it is not found on the object itself, JavaScript looks up its prototype.
- Then it looks at the prototype’s prototype, and so on…
- Until it reaches `null` — the end of the chain.

This allows objects to inherit properties and methods from their prototypes.

---
## Examples

```js
const obj = {};
console.log(obj.toString); 
// function toString() { ... } (from Object.prototype)

console.log(obj.__proto__ === Object.prototype); // true
console.log(obj.__proto__.__proto__ === null); // true
const arr = [];
console.log(arr.push); 
// function push() { ... } (from Array.prototype)

console.log(arr.__proto__ === Array.prototype); // true
console.log(arr.__proto__.__proto__ === Object.prototype); // true
```

---

#javascript