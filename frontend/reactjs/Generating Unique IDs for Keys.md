# Generating Unique IDs for Keys

Rather than use indexes as the keys in an array, it is better to generate unique IDs using the following method:

```jsx
function handleAddItem(value) {
  const nextItem = {
    id: crypto.randomUUID(),
    label: value,
  };

  const nextItems = [...items, nextItem];
  setItems(nextItems);
}
```

---

## API Example

```jsx
const [data, setData] = React.useState(null);

async function retrieveData() {
  const res = await fetch('/api/data');
  const json = await res.json();

  // The moment we have the data, we generate
  // an ID for each item:
  const dataWithId = json.data.map(item => {
    return {
      ...item,
      id: crypto.randomUUID(),
    };
  });

  // Then we update the state with
  // this augmented data:
  setData(dataWithId);
}
```

Source: [Common Beginner Mistakes with React](https://www.joshwcomeau.com/react/common-beginner-mistakes/)

---

#reactjs