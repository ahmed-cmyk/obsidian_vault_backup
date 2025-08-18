# React Components

React applications are built from isolated pieces of UI called **components**. A **React component** is a JavaScript function that you can sprinkle with a little bit of markup.

> **Note**: React components use a syntax extension called **JSX** to represent that markup. JSX looks a lot like HTML, but it is a bit stricter and can display dynamic information.

---

## Passing Props

Props are similar to HTML tags, except you can pass any JavaScript value through them, even JSX.

**Example**:

```jsx
<Avatar
	size={100}
	person={{
		name: 'Katsuko Saruhashi',
		imageId: 'YfeOqp2'
	}}
/>
```

**Source**: [Describing the UI | React](https://react.dev/learn/describing-the-ui)

---

## Rendering Lists

You can render lists using `.map()` or the `.filter()` methods in JavaScript.

**Example**:

```jsx
const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
 );
```

> **Note**: For each array item, you need to specify a `key`. This value must be unique. Specifying a key helps React keep track of each item’s place in the list, even if the list changes.

---

## Keeping Components Pure

>Some JavaScript functions are *pure.* Pure functions only perform a calculation and nothing more. By strictly only writing your components as pure functions, you can avoid an entire class of baffling bugs and unpredictable behavior as your codebase grows. To get these benefits, though, there are a few rules you must follow.

Pure functions have the following characteristics:

- **It minds its own business**. It does not change any objects or variables that existed before it was called.
- **Same inputs, same output**. Given the same inputs, a pure function should always return the same result.

> **Note**: React is designed around the concept of pure functions. **React assumes that every component you write is a pure function**. This means that React components you write must always return the same JSX given the same inputs.

> **Warning**: Don’t mutate objects or variables that existed prior to rendering. Doing so, can lead to unpredictable behavior.

### Local Variables

While it is against the rules to mutate variables that existed prior to rendering, you can still mutate variables that are created while rendering. These variables are referred to as **local variables**, and the reason why is because they exist only in the function were they are defined.

**Example**:

```jsx
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaGathering() {
  let cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```


### Input Variables in React

**In React, there are only three kinds of inputs that can be read while rendering:**

- `props`
- `state`
- `context`

> **Note**: These inputs should always be treated as **read-only**.

> **Add. Note**: If you wish to change any of the values, then you must **set state** instead of writing to a variable.

### Discovering Impure Components using Strict Mode

In strict mode, React calls each component function twice. Doing so helps discover impure variables as it breaks them in a way that is usually noticeable and obvious to the developer.

>**Note**: Strict mode has no effect in production, so it won’t slow down the app for users.

To opt into strict mode, you should wrap your root component into `<React.StrictMode>`.

### Side Effects in React

Sometimes you want certain side affects to occur after rendering. In React, side affects usually belong inside **event handlers**. These event handlers run when certain actions are taken, for example, when a button is clicked, when a form is submitted, etc.

> **Note**: These event handlers are defined inside your components, however, their existence is permitted because they don’t run during rendering.

If you are unable to find the right event handler for your side affect, then you could simply put it inside a `useEffect` call. This tells React to execute it later, after rendering.

---

## Why is Purity Important in React?

Writing pure functions takes some habit and discipline. But it also unlocks marvelous opportunities:

- Your components could run in a different environment—for example, on the server! Since they return the same result for the same inputs, one component can serve many user requests.
- You can improve performance by [skipping rendering](https://react.dev/reference/react/memo) components whose inputs have not changed. This is safe because pure functions always return the same results, so they are safe to cache.
- If some data changes in the middle of rendering a deep component tree, React can restart rendering without wasting time to finish the outdated render. Purity makes it safe to stop calculating at any time.

**Source**: [Keeping Components Pure | React](https://react.dev/learn/keeping-components-pure)

---
#reactjs