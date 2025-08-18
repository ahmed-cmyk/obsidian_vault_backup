# Async useEffect Function

You can’t define your `useEffect` hook as `async` and then expect it to work when fetching data from an API. To fetch data from an API inside of a `useEffect` hook, you should define an `async` function inside it.

```jsx
React.useEffect(() => {
  // Create an async function...
  async function runEffect() {
    const url = `${API}/get-profile?id=${userId}`;
    const res = await fetch(url);
    const json = await res.json();

    setUser(json);
  }

  // ...and then invoke it:
  runEffect();
}, [userId]);
```

> **Note**: `async` functions actually return a promise.

The `useEffect` hook expects us to return either nothing, or a cleanup function, not a promise.

**Source**: [Async useEffect function | Josh Comeau](https://www.joshwcomeau.com/react/common-beginner-mistakes/#async-effect-function-9)

---
#reactjs