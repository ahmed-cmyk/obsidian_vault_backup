# `useContext` in React

`useContext` is a React Hook that allows you to **read the current value of a Context** directly, without having to wrap your component in a `Consumer`.

> **NOTE**: It was introduced with Hooks in React 16.8, replacing the need for the older class-based `contextType` and `Consumer`pattern in functional components.

---
## Why use `useContext`?

In React, **Context** lets you pass data deeply through the component tree *without manually passing props at every level*.

Typical use cases:

* theme (light/dark)
* authenticated user
* language or localization
* feature flags

Before Hooks, you would write:

```jsx
<MyContext.Consumer>
  {value => <div>{value}</div>}
</MyContext.Consumer>
```

With Hooks, you simply write:

```jsx
const value = useContext(MyContext);
```

---
## Basic Example

### Step 1: Create the Context

```jsx
import { createContext } from "react";

const ThemeContext = createContext("light");
```

### Step 2: Provide a value higher in the tree

```jsx
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>
```

### Step 3: Use it in any child component

```jsx
import { useContext } from "react";

const MyComponent = () => {
  const theme = useContext(ThemeContext);
  return <div>Current theme: {theme}</div>;
};
```

---

## Key Notes

* You still need to wrap your tree with a `<Provider>` for the context to have a value.
* If no `value` is passed to the Provider, React will use the **default value** provided to `createContext()`.
* Calling `useContext(MyContext)` returns the **current context value**, and re-renders the component when the value changes.

---

## Common Pitfalls

* Do not call `useContext` outside of a React component or a custom hook.
* Do not overuse context for simple prop-passing scenarios — overusing it can make code harder to understand.
* `useContext` only subscribes to one context at a time. To use multiple contexts, call `useContext` for each one.

---

## When to Use

* Theme switching
* User authentication state
* Global configuration or feature flags
* Any global state that needs to be shared across many components

---
## Summary Table

| Topic | Details |
|---|---|
| What | React Hook to consume context |
| When | When you need to read global/shared state |
| Why | Cleaner and simpler than `<Consumer>` | 
| Where | Inside a React functional component |

---

#reactjs