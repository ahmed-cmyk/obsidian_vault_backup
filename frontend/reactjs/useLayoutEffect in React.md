# `useLayoutEffect` in React

`useLayoutEffect` is a React Hook similar to `useEffect`, but it fires **synchronously after all DOM mutations and before the browser paints**.

It allows you to **read layout and synchronously re-render** before the browser updates the screen.

---
## Why use `useLayoutEffect`?

* When you need to **measure DOM elements** and adjust layout **before paint**.
* To avoid visual flicker between renders and browser paint.
* When performing operations that must block painting until complete.

---
## Basic Example

### Syntax:

```jsx
useLayoutEffect(() => {
  // effect code
  return () => {
    // cleanup
  };
}, [dependencies]);
```

### Example:

```jsx
import { useLayoutEffect, useRef } from "react";

function Component() {
  const boxRef = useRef();

  useLayoutEffect(() => {
    const { height } = boxRef.current.getBoundingClientRect();
    console.log("Box height:", height);
  }, []);

  return <div ref={boxRef}>Measure me</div>;
}
```

---
## Key Notes

* Runs **after DOM updates** but **before paint**.

* Blocks painting until it finishes — be careful of performance issues.

* Same cleanup behavior as `useEffect`.

---
## `useEffect` vs `useLayoutEffect`

| Feature | `useEffect` | `useLayoutEffect` | 
|---|---|---|
| When it runs | After paint | Before paint |
| Blocking? | No | Yes |
| Use case | Async side-effects | Synchronous DOM reads/writes |

---

## When to Use

* Measuring DOM nodes (`getBoundingClientRect`).
* Synchronizing animations with layout.
* Avoiding content jump / flicker when adjusting layout.

---

#reactjs