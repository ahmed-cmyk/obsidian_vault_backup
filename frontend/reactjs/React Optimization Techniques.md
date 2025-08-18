# React Optimization Techniques

Optimizing React applications improves performance, user experience, and scalability. Below are the core techniques you should know, with additional insights and examples where useful.

---
## 1. Memoization: `React.memo`, `useCallback`, `useMemo`

**Purpose:** Avoid unnecessary re-renders by reusing previous results.

* **`React.memo`**
* Wraps functional components to skip rendering when props have not changed (shallow comparison).

```jsx
const MyComponent = React.memo(function MyComponent(props) {
  return <div>{props.value}</div>;
});
```

* **`useCallback`**
* Memoizes a callback function between renders if dependencies don’t change.
* Use it when passing callbacks to child components that depend on reference equality for `React.memo` to work.

```jsx
const handleClick = useCallback(() => {
  console.log('Clicked');
}, [dependency]);
```

* **`useMemo`**
* Memoizes expensive calculations and avoids recomputation unless dependencies change.

```jsx
const memoizedValue = useMemo(() => expensiveCalculation(a, b), [a, b]);
```

Be mindful of overusing memoization — it adds complexity and consumes memory. Use it only when necessary.

---
## 2. Virtualization for Large Lists

**Purpose:** Rendering large lists all at once can overwhelm the DOM and degrade performance.

**Solution:** Render only what is visible in the viewport and dynamically load or unload others as the user scrolls.

**Tools:**

* `react-window`
* `react-virtualized`

---
## 3. Lazy Loading & Code Splitting

**Purpose:** Reduce the initial bundle size by loading components or routes only when they are needed.

* **`React.lazy` + `Suspense`**

```jsx
const OtherComponent = React.lazy(() => import('./OtherComponent'));

<Suspense fallback={<div>Loading...</div>}>
  <OtherComponent />
</Suspense>
```

Combine with dynamic imports and tools like Webpack or Vite for code splitting.

---
## 4. Optimize State Management

**Purpose:** Minimize unnecessary updates to state that trigger re-renders.

* Group related state together.
* Keep state as close as possible to the components that use it.
* Use `useReducer` for complex state logic.
* Avoid setting state if the new value equals the current value.
* Use libraries like Redux, Zustand, or Jotai for predictable, scalable global state.

---
## 5. Optimize Images & Media

* Use modern formats like WebP or AVIF.
* Compress assets.
* Use responsive images with `srcset` or `<picture>`.
* Lazy-load images with `loading="lazy"` or the Intersection Observer API.
* Serve appropriately sized assets for each device.

---
## 6. Avoid Prop Drilling

**Purpose:** Passing props through many layers adds complexity and tightly couples components.

**Solution:**

* Use the Context API for infrequently changing global data.
* Use Redux, Zustand, or other libraries for frequent or complex state needs.

---
## 7. Use Stable Keys in Lists

React uses keys to determine which elements changed. Keys must be unique and stable across renders.

**Bad:**

```jsx
{items.map((item, index) => <div key={index}>{item}</div>)}
```

**Good:**

```jsx
{items.map(item => <div key={item.id}>{item.value}</div>)}
```

---
## 8. Debouncing & Throttling

Heavy event handlers like scroll, resize, or input can overload the browser.

* **Debounce:** Trigger a function only after the user stops triggering events for a defined period.
* **Throttle:** Limit how often a function can run over time.

**Libraries:**

* Lodash (`_.debounce`, `_.throttle`)
* Custom implementations

---
## 9. Profile & Analyze

Measure and pinpoint performance problems before optimizing.

* **React DevTools Profiler**
* Lighthouse
* Web Vitals (Largest Contentful Paint, Cumulative Layout Shift, First Input Delay)
* Bundle analyzers (Webpack Bundle Analyzer)

---
## 10. SSR & SSG

**Purpose:** Improve SEO and perceived performance for content-heavy apps.

* Server-Side Rendering (SSR) — e.g., with Next.js
* Static Site Generation (SSG) — e.g., with Next.js, Gatsby

SSR and SSG reduce Time to First Byte (TTFB) and improve crawlability by search engines.

---
## 11. Optimize CSS & Styling

* Minimize reflows and repaints by batching DOM updates.
* Use `will-change` for animations.
* Prefer CSS classes over dynamic inline styles.
* Extract critical CSS to speed up the first paint.
* Consider CSS modules, Tailwind, or other CSS-in-JS solutions cautiously — they have trade-offs.

---
## 12. Work with Immutable Data

React depends on immutability to detect changes.

**Bad:**

```jsx
arr.push(4);
```

**Good:**

```jsx
const newArr = [...arr, 4];
```

Use libraries like `Immer` for concise immutable updates.

---
## 13. Avoid Inline Functions & Objects in Render

Inline definitions create new references every render, defeating memoization.

**Bad:**

```jsx
<Component onClick={() => console.log('click')} />
```

**Good:**

```jsx
const handleClick = useCallback(() => console.log('click'), []);
<Component onClick={handleClick} />
```

---
## 14. Use Web Workers for Heavy Computations

Offload CPU-intensive tasks from the main thread to keep the UI responsive.

**Tools:**

* `worker-loader` for Webpack
* native Web Worker API

---
## 15. Use React Fragments

Eliminate unnecessary DOM nodes.

```jsx
<>
  <Child1 />
  <Child2 />
</>
```

---
## 16. Memoize Selectors in State Management

If you use Redux or similar, memoized selectors prevent recomputations and unnecessary renders.

**Tool:**

* `reselect`

---
# Additional Best Practices

* **Split bundles by route:** Use dynamic imports for route-level code splitting.
* **Minimize render depth:** Avoid deep component trees when possible.
* **Batch state updates:** React automatically batches updates, but avoid multiple independent state calls in quick succession.
* **Preconnect and prefetch:** Use `<link rel="preconnect">` and `<link rel="prefetch">` to load resources sooner.

---
# Common Mistakes to Avoid

* Overusing `useMemo` and `useCallback` unnecessarily — they have a cost.
* Relying on unstable keys in lists (like array indices).
* Not profiling before optimizing — premature optimization can harm readability.
* Keeping too much state in the global scope when local state suffices.
* Not cleaning up effects in `useEffect`.
* Passing anonymous functions or objects directly to child components.

---
# Summary Checklist

* Use `React.memo`, `useCallback`, and `useMemo` when appropriate.
* Virtualize long lists (`react-window`, `react-virtualized`).
* Lazy load components and implement code splitting.
* Optimize state and use it wisely (local, global, reducer).
* Compress and optimize media assets.
* Avoid prop drilling by using Context or state managers.
* Use stable, unique keys in lists.
* Debounce or throttle heavy event handlers.
* Profile and measure performance before optimizing.
* Leverage SSR or SSG for SEO and faster page loads.
* Minimize CSS and avoid layout thrashing.
* Work immutably with state.
* Avoid inline functions and objects in render.
* Use Web Workers for heavy processing.
* Use React Fragments instead of unnecessary divs.
* Memoize selectors if using Redux or similar.
* Split bundles by route.
* Batch and minimize state updates.
* Clean up side effects in `useEffect`.

---

#reactjs