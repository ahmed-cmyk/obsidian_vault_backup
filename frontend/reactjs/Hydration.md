# Hydration

If you wish to conditionally render HTML fields based on whether a particular state condition has been met then you should avoid using hack-y solutions like the following. 

```jsx
const Navigation = () => {
  if (typeof window === 'undefined') {
    return null;
 }
```

> **Note**: The logic behind this code is so that we can detect whether or not we're rendering on the server by checking to see if window exists. If it doesn't, we can abort the render early.
> 
> The problem with this approach is that when using SSR, React checks to see if the virtual DOM tree matches the tree generated during the server compilation. Performing such checks means that there will be a difference.

The following example showcases a scenario of a user component where if the user is currently logged in, they are shown a completely different view than they a logged-out user.

**Example:**

```jsx
function Navigation() {
  const [hasMounted, setHasMounted] = React.useState(false);

  React.useEffect(() => {
    setHasMounted(true);
  }, []);

  if (!hasMounted) {
    return null;
  }

  const user = getUser();

  if (user) {
    return (
      <AuthenticatedNav
        user={user}
      />
    );
  }

  return (
    <nav>
      <a href="/login">Login</a>
    </nav>
  );
};
```

> The `useEffect` hook only fires when the DOM has been mounted, meaning it won’t get called during hydration, hence, we are meeting React’s expectations.

> **Note**: This technique is referred to as **two-pass rendering**.

**Two-pass rendering** is the same idea. The first pass, at compile-time, produces all of the static non-personal content, and leaves holes where the dynamic content will go. Then, after the React app has mounted on the user's device, a second pass stamps in all the dynamic bits that depend on client state.

---

## Performance Implications

The downside to two-pass rendering is that it can **delay time-to-interactive**. Forcing a render right after mount is generally frowned upon.

That said, for most applications, this shouldn't make a big difference. Usually the amount of dynamic content is relatively small, and can be quickly reconciled. If huge chunks of your app are dynamic, you'll miss out on many of the benefits of pre-rendering, but this is unavoidable; dynamic sections can't be produced ahead of time by definition.

**Source**: [Hydration Perils | Josh Comeau](https://www.joshwcomeau.com/react/the-perils-of-rehydration/)

---
#reactjs