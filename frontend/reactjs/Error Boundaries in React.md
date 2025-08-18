# Error Boundaries in React

## What is an Error Boundary?

An **Error Boundary** is a React component that catches JavaScript errors anywhere in its child component tree, logs those errors, and displays a fallback UI instead of crashing the whole application.

They let you gracefully handle errors that occur during **rendering**, in **lifecycle methods**, and in **constructors** of the child components.

> **NOTE**: Without an error boundary, React will unmount the entire component tree below the component where the error occurred.

---
## How to Create an Error Boundary

You create an Error Boundary by defining a class component that implements either (or both) of these lifecycle methods:

* `static getDerivedStateFromError(error)` — allows you to render a fallback UI.
* `componentDidCatch(error, info)` — allows you to log the error.

```jsx
import React from "react";

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.error("Error caught by ErrorBoundary:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

You can then wrap any part of your app with it:

```jsx
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

---
## When to Use Error Boundaries?

* When integrating third-party components you don’t fully control.
* To protect parts of the app that deal with unreliable external data.
* Around large sections of the app (e.g., page-level) to prevent the whole app from breaking.

---
## What Errors Do Error Boundaries Catch?

✅ Rendering errors in:

* Child components
* Their lifecycle methods
* Their constructors

🚫 They **do not catch**:

* Errors inside event handlers
* Errors in asynchronous code (like `setTimeout`)
* Errors in server-side rendering
* Errors thrown in the ErrorBoundary itself

For event handlers, you handle errors manually in a `try...catch`.

---
## Best Practices

* Use at page/route level or around significant modules rather than wrapping every single component.
* Customize the fallback UI for better UX, like offering a retry button or redirect.
* Log errors to a monitoring service in `componentDidCatch`.

---
## Common Mistakes

* Expecting Error Boundaries to catch async errors — they don’t.
* Forgetting to reset `hasError` state if you want to recover without a full page reload.

---

#reactjs