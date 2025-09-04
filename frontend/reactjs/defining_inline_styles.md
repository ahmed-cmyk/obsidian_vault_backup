# Defining inline styles

In JSX, you need to define inline styles using double curly braces and camelCased property names. An example of this is presented below:

```jsx
<button
  // "{{", instead of "{":
  style={{ color: 'red', fontSize: '1.25rem' }}
>
  Hello World
</button>
```

> **Note**: If you accidentally use single curly braces, then that is wrong because it denotes an expression slot.

**Source**: [Defining inline styles | Josh Comeau](https://www.joshwcomeau.com/react/common-beginner-mistakes/#missing-style-brackets-8)

---

#reactjs