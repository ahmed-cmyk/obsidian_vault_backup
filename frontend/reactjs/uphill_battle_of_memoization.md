# Uphill Battle of Memoization

If we wrap `React.memo` around a component, React will avoid rendering that component until its props change. This behavior makes it easier to break.

> **Note**: If you add a `style` prop to a memoized component, you essentially break the render as it will be a new object on every render.
> 
> **Add. Note**: You can fix that by memoizing your `style` props, however, it makes our code harder to reason about.

---

## `{children}`

Memoized components that take in `{children}` are also problematic as they can break the memoization of a component.

The reason why this happens is because of `React.createElement`, which creates a new object on every render. These objects might look the same to us, but the reference isn’t the same.

**Source**: [The Uphill Battle of Memoization | TkDodo](https://tkdodo.eu/blog/the-uphill-battle-of-memoization?utm_source=reactdigest&utm_medium&utm_campaign=1720)

---
#reactjs