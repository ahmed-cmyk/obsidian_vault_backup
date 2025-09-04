# How re-renders work

Re-renders only affect the component that owns the updated state, and any of its descendants. Whenever a state changes, it prompts react to create another render.

Each render, is essentially a **snapshot**, which react compares with the current render, to see what has changed. Any nodes that have changed are replaced with the newer node.

> **Note**: Data can’t flow up in a reactjs application, hence, there is no need to re-render the parents of the component that owns the state.

React needs to re-render all components even if the state tied to them has not updated in an attempt to ensure that everything remains up-to-date. There is no mechanism for it to determine by itself whether or not a component should be re-rendered.

> In an ideal world, React components would always be “pure”. A pure component is one that always produces the same UI when given the same props.

---

## Create Pure Components

To create a pure component you could use:

- `React.memo`
- `React.PureComponent`

There are multiple methods to use `React.memo` in your component. You could choose to wrap the tags around your component:

```jsx
<React.memo>
	<yourComponent />
</React.memo>
```

Or, you could apply it to your import statement:

```jsx
export default React.memo(yourComponent);
```

> Using one of these tells react that the UI component is pure and that there is no need to re-render the component unless its props change. This technique is referred to as `memoization`.

> It's missing the R, but we can sorta think of it as “memorization”. The idea is that React will remember the previous snapshot. If none of the props have changed, React will re-use that stale snapshot rather than going through the trouble of generating a brand new one.

> **Warning**: Don’t start turning every component into a pure component just because you can. Developers tend to overestimate how expensive re-renders can be. Sometimes it can actually be slower to check if any of the props have changed compared to re-rendering the component.

---

## Context

Contexts created using the `useContext` hook act as invisible props in a pure component. It triggers a re-render when changed but only when it actually gets consumed.

**Source**: [React re-renders | Josh Comeau](https://www.joshwcomeau.com/react/why-react-re-renders/)

---
#reactjs