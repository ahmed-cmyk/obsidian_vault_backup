# `useReducer` in React

`useReducer` is a React Hook that is an alternative to `useState`, useful when the state logic is complex or when the next state depends on the previous one.
It also makes it easier to manage state transitions in a predictable way by centralizing them in a **reducer function**.

---
## Why use `useReducer`?

* Useful when state has **multiple sub-values**.
* Useful when **next state depends on previous state**.
* Makes logic more explicit and maintainable — especially for more complex components.
* Can make testing and refactoring easier.

---
## Basic Example

### Syntax:

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

### Example:

```jsx
import { useReducer } from "react";

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </>
  );
}
```

---
## Key Notes

* Similar to `Redux` pattern but scoped to a single component.
* `state` is the current state.
* `dispatch` is a function you call with an action object.
* The `reducer` is a pure function: `(state, action) => newState`.
* Unlike `useState`, you don’t update the state directly.

---
## When to Use

* Multiple related state variables.
* State logic is too complex for `useState`.
* When state transitions are better expressed as a series of actions.

---

#reactjs