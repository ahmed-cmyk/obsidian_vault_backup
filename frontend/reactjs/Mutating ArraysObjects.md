# Mutating Arrays/Objects

```jsx
const [items, setItems] = React.useState([
// items
]);
```

The wrong way to mutate this array is as follows:

```jsx
items.push(item);
```

The reason why this is wrong is because react relies on a state variable’s identity in order to know if it has been changed. Here, we aren’t changing its identity.

> **Note**: This also breaks the most sacred rule in react which is that we should not mutate state directly.

The right way to mutate an array, is to create a new array and then set the value to that of the new array.

```jsx
function handleAddItem(value) {
  const nextItems = [...items, value];
  setItems(nextItems);
}
```

The exact same concept applies to objects.

```jsx
function handleChangeEmail(email) {
  const nextUser = { ...user, email: nextEmail };
  setUser(nextUser);
}
```

Source: [Common Beginner Mistakes with React](https://www.joshwcomeau.com/react/common-beginner-mistakes/)

---

#reactjs