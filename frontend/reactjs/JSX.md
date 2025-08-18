# JSX

JSX is a syntax extension to React. The following code is an example of a variable declaration in JSX:

```jsx
const element = <h1>Hello, World!</h1>;
```

> **Note**: React doesn’t require the use of JSX, but most people find it helpful as a visual aid when working with UI inside the JavaScript code.

You can put normal JavaScript expressions inside curly braces.

```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

> **Note**: JSX is an expression too so it can be used with JavaScript if-else statements for **conditional rendering**.

> **Add. Note**: You can also use curly braces to place JavaScript expressions in an attribute. For example, you could put a JavaScript expression inside the `src` attribute for an `<img>` tag.

> **Imp.**: Since JSX is closer to JavaScript than to HTML, React DOM uses camelCase property naming convention instead of HTML attribute names.

Babel compiles JSX down to `React.createElement()` calls. Essentially, it creates something similar to the following:

```jsx
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

Out of:

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

> **Note**: The objects created by `React.createElement()` are called **React Elements**, and React reads and uses them to construct the DOM and keep it up-to-date.

---

## XSS Protection

By default, React DOM escapes any values embedded in the JSX before rendering them. Thus, it ensures that you can never inject anything that’s not been explicitly written in your application.

> **Note**: Everything is converted into a string before it is rendered.

**Source**: [Introducing JSX | React](https://legacy.reactjs.org/docs/introducing-jsx.html)

---
#reactjs