# Code Splitting & Lazy Loading in Depth

## What is Code Splitting?

Code splitting is the practice of breaking your JavaScript bundle into smaller chunks that can be loaded on demand rather than delivering one large file to the browser.

### Why?

* Reduces initial load time.
* Faster Time-to-Interactive (TTI).
* Improves perceived performance.

> **NOTE**: In React, code splitting is commonly done at the **route level**, but it can also be applied to components or libraries that are not needed immediately.

---
## Techniques for Code Splitting

### 1. Dynamic `import()`

JavaScript provides `import()` for loading modules dynamically.
Webpack and other bundlers use this to generate separate chunks.

```jsx
import('./MyComponent').then((module) => {
  const MyComponent = module.default;
});
```

---
### 2. `React.lazy()`

React provides `React.lazy()` to lazily load components as separate chunks.

```jsx
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

> **Note:** You must wrap lazy components in `<Suspense>` and provide a fallback.

---
## When to Use Code Splitting?

✅ When you have heavy routes/pages/components that are not needed immediately.
✅ When using libraries that are large and used sparingly (e.g., charting libraries).
✅ To defer loading admin or settings pages until the user navigates there.

---
## Best Practices

* Split by **route/pages** as the primary strategy.
* Avoid over-splitting — too many requests can hurt performance.
* Use **named chunks** when configuring Webpack for easier debugging.
* Use tools like `webpack-bundle-analyzer` to visualize bundle size.

---
## Common Pitfalls

🚫 Forgetting to provide a fallback in `<Suspense>`.
🚫 Code splitting too aggressively — leading to many tiny files and increased latency.
🚫 Not testing fallback UI thoroughly — users may see an empty screen during load.
🚫 Dynamic imports that fetch code repeatedly if not cached or memoized.

---

## Beyond Components: Lazy Loading Images & Assets

In addition to code, you can also lazily load:

* Images: `<img loading="lazy" />` or libraries like `react-lazyload`.
* Fonts: use `font-display: swap` in CSS.
* Non-critical CSS: use `media="print"` trick or async CSS loaders.

---

Code splitting and lazy loading, when applied correctly, significantly improve user experience by delivering only what’s necessary, when it’s necessary.

---

#reactjs