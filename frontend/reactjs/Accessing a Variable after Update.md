# Accessing a Variable after Update

The following code attempts to print out the value of count to the console right after it has been updated. The issue with this approach is that it assumes that the `setter` method in react functions as an assignment. In actuality, we are setting an update.

What this means is that the `console.log` command will most likely print out the previous value instead of the new and updated value.

```jsx
function handleClick() {
  setCount(count + 1);
  console.log({ count });
}
```

To remedy that it is better to store the updated value in a new variable and then access that variable. Like so:

```jsx
function handleClick() {
  const nextCount = count + 1;
  setCount(nextCount);
  console.log({ nextCount });
}
```

Source: [Accessing State | Josh Comeau](https://www.joshwcomeau.com/react/common-beginner-mistakes/#accessing-state-after-changing-it-5)

---

#reactjs