# Adding Interactivity

**Event handler functions**:

- Are usually defined *inside* your components.
* Have names that start with handle, followed by the name of the event.

Functions should be passed into event handlers, not called.

```jsx
<button onClick={handleClick}> // Passing
<button onClick={handleClick()}> // Calling
```

If you call a function inside an event handler, then it will fire as soon as the component renders.

Alternatively, if you wish to execute inline code instead of passing a function, then you should wrap it into an anonymous function, like so:

```jsx
<button onClick={() => alert('You clicked me!')}>
```

---

## Event Propagation

Event handlers will also catch events from any children your component might have. We say that an event “bubbles” or “propagates” up the tree: it starts with where the event happened, and then goes up the tree.

```jsx
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <button onClick={() => alert('Playing!')}>
        Play Movie
      </button>
      <button onClick={() => alert('Uploading!')}>
        Upload Image
      </button>
    </div>
  );
}
```

If you click on either button, its onClick will run first, followed by the parent <div>’s onClick. So two messages will appear. If you click the toolbar itself, only the parent <div>’s onClick will run.

>**Note**: All events propagate in React except onScroll, which only works on the JSX tag you attach it to.

### Stopping Propagation

Event handlers receive an **event object** as their only argument. By convention, it’s usually called `e`, which stands for event. You can use this object to read information about the event.

Using that event object, you can stop propagation by calling `e.preventPropagation()`.

**Example**:

```jsx
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <Button onClick={() => alert('Playing!')}>
        Play Movie
      </Button>
      <Button onClick={() => alert('Uploading!')}>
        Upload Image
      </Button>
    </div>
  );
}
```

> The `e.preventPropagation()` call prevents the event from bubbling.

### Preventing Default Behavior

Certain elements have default behaviors that run when a user interacts with them. For example, a form will reload the whole page after being submitted.

To prevent these default behaviors, we can use the `e.preventDefault()` call.

---

## State

**To update a component with new data, two things need to happen**:

- **Retain** the data between renders.
- **Trigger** React to render the component with new data (re-rendering).

**The `useState` Hook provides those two things**:

- A **state variable** to retain the data between renders.
- A **state setter function** to update the variable and trigger React to render the component again.

**Adding a state variable**:

```jsx
import { useState } from 'react';

const [index, setIndex] = useState(0);
```

> **Note**: The `[` and `]` syntax here is called [array destructuring](https://javascript.info/destructuring-assignment) and it lets you read values from an array. The array returned by useState always has exactly two items.

> **Add. Note**: Updating your component’s state, automatically queues a re-render.

### Updater Functions

Under the hood, the `setState` function actually passes an argument which allows the state change to take place. The following code:

```react
setIndex(5);
```

Actually works like this:

```react
setIndex(n => 5)
```

> **Note**: The name of the argument doesn’t need to be `n`, you can set it to whatever you like. However, it is common to name the argument by the first letters of the state variable that you are updating.

Using, that syntax you can create an updater function:

```react
setIndex(n => n + 1);
```

### Update Multiple Object Fields with a Single Event Handler

```react
setPerson({
	...person,
	[e.target.name]: e.target.value
});
```

> **Note**: The square brackets (`[]`) are used to specify a property with a dynamic name.

### Updating Arrays

**The following functions return a new array whenever they are called**:
| **Operation** | **prefer (returns a new array)** |
|-|---|
| adding |concat, [...arr] spread syntax ([example](https://react.dev/learn/updating-arrays-in-state%23adding-to-an-array)) |
| removing |filter, slice ([example](https://react.dev/learn/updating-arrays-in-state%23removing-from-an-array)) |
| replacing |map ([example](https://react.dev/learn/updating-arrays-in-state%23replacing-items-in-an-array)) |
| sorting | copy the array first ([example](https://react.dev/learn/updating-arrays-in-state%23making-other-changes-to-an-array)) |

---

## Hooks

In React, functions that start with `use` are called Hooks. Hooks are special functions that are only available when React is rendering.

> **Note**: Hooks can only be called at the top level of components or your own Hooks. You can’t call Hooks inside conditions, loops, or other nested functions.

---

## Rendering

**The process of requesting and serving the UI has three steps**:

1. Triggering a render
2. Rendering the component
3. Committing to the DOM

**There are two reasons for a component to render**:

1. It is the component’s initial render.
2. The component’s (or one of its ancestors’) state has been updated.

The initial render is done by calling `createRoot` with the target DOM node, and then calling its `render` method with your component.

```jsx
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);
```

For the initial render, React will use the `appendChild()` DOM API to put all the DOM nodes it has created on the screen.

> The `appendChild()` method adds a node to the end of the list of children of a specified parent node.

Subsequent re-renders are performed automatically, and they happen whenever a `set` function is called. For example, when a `setName` function is called. For re-renders, react only updates the elements that it deems necessary instead of every single element on the component.

> If the updated component nests another component, then that component will also be re-rendered.

---

## Source(s)

[Adding Interactivity | React](https://react.dev/learn/responding-to-events)
[State: A Component's Memory | React](https://react.dev/learn/state-a-components-memory)
[Render and Commit | React](https://react.dev/learn/render-and-commit)

---
#reactjs