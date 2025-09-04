# Reconciliation & React Fiber

## Reconciliation

Reconciliation is the process React uses to determine how the Virtual DOM should be updated to match a new state or props.

When you change the state of a component, React doesn’t replace the whole DOM tree. Instead, it compares the new Virtual DOM tree to the previous one and updates only the parts of the real DOM that changed. This process is called **diffing**.

### Key Concepts:

* **Virtual DOM**: A lightweight copy of the real DOM kept in memory.
* **Diffing Algorithm**: React’s algorithm compares two Virtual DOM trees in a very efficient way:
  * It assumes elements of different types produce different trees.
  * When comparing lists, it uses the `key` prop to identify which items changed, were added, or removed.
  * Without `key`, React will re-render all items unnecessarily.

### Why `key` Matters:

Keys help React identify which items in a list have changed. Without them, React cannot properly reuse DOM nodes, leading to unnecessary re-renders and worse performance.

```jsx
{items.map(item => (
  <li key={item.id}>{item.text}</li>
))}
```

---
## React Fiber

React Fiber is the **reimplementation of React’s core algorithm** starting from React 16. It enables:

* Incremental rendering: breaking work into chunks and spreading it over multiple frames.
* Better prioritization: more important updates (like input responsiveness) are processed before less urgent ones.
* Ability to pause, resume, and abort work — enabling features like Suspense and concurrent rendering.

### Key Benefits:

* No longer blocks the main thread with large updates.
* Supports features like **Concurrent Mode**, **Suspense**, and **startTransition**.
* Prepares React for gradual and interruptible rendering.

Fiber breaks the work of rendering into **units of work**, which can be paused and resumed between frames, keeping apps responsive.

---
## Common Mistakes to Avoid:

- Forgetting to add unique `key` props in lists — this breaks the diffing and causes full re-renders.
- Using indexes as keys — only acceptable if list items never change order or content.
- Assuming React always re-renders everything — it only updates what’s necessary.
- Manipulating the DOM directly — bypasses React’s Virtual DOM and can lead to inconsistencies.

---

#reactjs