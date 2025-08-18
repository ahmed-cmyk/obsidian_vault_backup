# Portals in React

## What is a Portal?

A **Portal** provides a way to render a child component **into a DOM node outside the current component hierarchy** — while still preserving React’s event bubbling and context.
This is particularly useful when rendering UI elements that need to visually break out of their parent container, such as **modals, tooltips, popovers, and dropdowns**.

---
## Why Use Portals?

Sometimes, CSS or z-index constraints in the component tree prevent an element (like a modal) from displaying correctly.
Portals let you escape the parent DOM structure and render into another part of the DOM (commonly a `<div id="modal-root"></div>` directly under `<body>`).

---
## How to Create a Portal

React provides the `ReactDOM.createPortal` method.

**Basic Example:**

```jsx
import React from "react";
import ReactDOM from "react-dom";

function Modal({ children }) {
  return ReactDOM.createPortal(
    <div className="modal">{children}</div>,
    document.getElementById("modal-root")
  );
}

// Usage
function App() {
  return (
    <div>
      <h1>My App</h1>
      <Modal>
        <h2>This is rendered in a portal!</h2>
      </Modal>
    </div>
  );
}
```

Here:

* The `Modal` component renders its children inside `#modal-root`, which exists **outside** the normal React root.
* Despite rendering elsewhere in the DOM, React still maintains event bubbling, context, and state correctly.

---
## Common Use Cases

✅ Modals
✅ Tooltips / Popovers
✅ Dropdown menus
✅ Notifications / Toasts
✅ Overlays and Backdrops

---
## Best Practices

* Make sure to include the target DOM node (`#modal-root`) in your HTML.
* Ensure proper focus management and accessibility (e.g., trap focus in modals).
* Don’t overuse portals unnecessarily — use them only when CSS or layout constraints make it impossible to keep the element in the normal tree.

---
## Common Mistakes

* Forgetting to create and include the `modal-root` element in `index.html`.
* Expecting portals to solve all z-index issues — they help but proper CSS is still needed.
* Using portals when not required — if you can solve layout issues within the normal DOM hierarchy, do that first.

---

#reactjs