# Escape Hatches

## Remembering Values with Refs

When you want a component to remember some information, but you don’t want that information to trigger new renders, you can use a `ref`.

To add a `ref` to your component, you need to import `useRef` from React:

```jsx
import { useRef } from 'react';
```

Inside your component, call the `useRef` Hook and pass the initial value that you want to reference as the only argument.

```jsx
const ref = useRef(0);
```

`useRef` returns an object like:

```jsx
{
	current: 0
}
```

> **Note**: You can access the current value of ref through the `ref.current` property. This value is mutable, which means you can read and write to it.

> **Add. Note**: ref can point to just about anything: a string, an object, or even a function.

> **Add. Note**: When a piece of information is used for rendering, keep it in state. When a piece of information is only needed by event handlers and changing it doesn’t require a re-render, using a ref might be more efficient.

### Differences between refs and state

| **refs**                                                     | **state**                                                    |
|:------------------------------------------------------------:|:------------------------------------------------------------:|
| `useRef(initialValue)` returns `{ current: initialValue }`   | `useState(initialValue)` returns the current value of a state variable and a state setter function ( `[value, setValue]`) |
| Doesn’t trigger re-render when you change it.                | Triggers re-render when you change it.                       |
| Mutable—you can modify and update current’s value outside of the rendering process. | “Immutable”—you must use the state setting function to modify state variables to queue a re-render. |
| You shouldn’t read (or write) the current value during rendering. | You can read state at any time. However, each render has its own [snapshot](https://react.dev/learn/state-as-a-snapshot) of state which does not change. |

### Internals

You can imagine `useRef` is implemented like this inside of React:

```jsx
// Inside of React
function useRef(initialValue) {
  const [ref, unused] = useState({ current: initialValue });
  return ref;
}
```

### When to use refs

- Storing timeout IDs
- Storing and manipulating DOM elements
- Storing other objects that aren’t necessary to calculate the JSX

### Manipulating the DOM with Refs

Sometimes you need access to the DOM nodes managed by React — for example, to focus a node, scroll to it, or measures its size and position. There is no built-in way to do these things in React, so you will need a `ref` to the DOM node.

To attach `ref` to a node:

```jsx
import { useRef } from 'react';
```

```jsx
const myRef = useRef(null) // declare the ref inside your component
```

```jsx
<div ref={myRef} />
```

> **Note**: When React creates a DOM node for this `<div>`, React will put a reference to this node into `myRef.current`. You can then access this DOM node from your event handlers and use the built-in browser APIs defined on it.

#### Accessing another component’s DOM nodes

Normally, you are unable to simply attach a `ref` to another component and then expect it to work, even if you pass the `ref` as a prop. Instead, there’s another step that needs to be taken, which is that you need to opt into that behavior using the `forwardRef` API:

```jsx
const MyInput = forwardRef((props,ref) => {
	return <input {...props} ref={ref} />;
});
```

> **Note**: Sometimes you want to restrict the exposed functionality. To do that, you can use the `useImperativeHandle` hook.

An example of the `useImperativeHandle` hook in use:

```jsx
const MyInput = forwardRef((props, ref) => {
  const realInputRef = useRef(null);
  useImperativeHandle(ref, () => ({
    // Only expose focus and nothing else
    focus() {
      realInputRef.current.focus();
    },
  }));
  return <input {...props} ref={realInputRef} />;
});
```

> **Imp**: *When using `refs` avoid changing DOM nodes managed by React*. Modifying, adding children to, or removing children from elements that are managed by React can lead to inconsistent visual results or crashes.
> 
> However, this doesn’t mean you can’t do it at all. *You can safely modify parts of the DOM that React has no reason to update*.

---

## Effects

Effects let you run code after rendering so that you can synchronize your component with some system outside of React.

**There are three steps to follow when writing an Effect**:
1. Declare an Effect
2. Specify the Effect dependencies
3. Add cleanup if needed

To declare an Effect, import the `useEffect` hook from React:

```jsx
import { useEffect } from 'react';
```

Then, call it from the top-level of your component and put some code inside your Effect:

```jsx
function MyComponent() {
  useEffect(() => {
    // Code here will run after *every* render
  });
  return <div />;
}
```

> **Note**: By default, the `useEffect` hook will re-run during every render. You have to use dependencies to prevent that.

You can add an dependencies to your `useEffect` hook so that it only runs when one of its dependencies updates:

```jsx
function MyComponent() {
  useEffect(() => {
    // Code here will run after *every* render
  }, [yourDepency]);
  return <div />;
}
```

> **Note**: React compares the dependency values using the [Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)

There are certain differences between how the `useEffect` hook acts based on whether you have a dependency array, or not, and if it has been left empty:

```jsx
useEffect(() => {
  // This runs after every render
});

useEffect(() => {
  // This runs only on mount (when the component appears)
}, []);

useEffect(() => {
  // This runs on mount *and also* if either a or b have changed since the last render
}, [a, b]);
```

> **Imp**: You cannot pass `ref` as one of your dependencies as it has a *stable identity* meaning that it will never change, hence, it will never cause the effect to re-run.
> 
> The reason why this is the case is because React guarantees that you’ll always get the same object from the same `useRef` call on every render.

> **Add Imp.**: The [set functions](https://react.dev/reference/react/useState%23setstate) returned by useState also have stable identity, so you will often see them omitted from the dependencies too.

### Cleanup Functions

> **Note**: React runs `useEffect` hooks twice during development. Due to this, some issues might crop up as some effects might break if called more than once. To deal with this you should probably look into cleanup functions.

**The syntax for a cleanup function is as follows**:

```jsx
function MyComponent() {
  useEffect(() => {
    // Code here will run after *every* render
	return () => {
	// cleanup code
	}
  }, [yourDepency]);
  return <div />;
}
```

> **Note**: Despite how wonderful Effects are, if there is no external system, then there you shouldn’t need them. Removing unnecessary Effects makes your code easier to follow, faster to run, and less error-prone.
 
## When you don’t need Effects

**Cases where you don’t need Effects**:
- You don’t need Effects to transform data for rendering
- You don’t need Effects to handle user events

### Updating State based on Props or State

The following is a good practice as it calculates the value of the State during rendering:

```jsx
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');
  // ✅ Good: calculated during rendering
  const fullName = firstName + ' ' + lastName;
  // ...
}
```

If you wish to update a list based on a filter or another condition, then it is best to memoize the operation as list changes tend to be expensive:

```jsx
import { useMemo, useState } from 'react';

function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  const visibleTodos = useMemo(() => {
    // ✅ Does not re-run unless todos or filter change
    return getFilteredTodos(todos, filter);
  }, [todos, filter]);
  // ...
}
```

> **Note**: The arguments surrounded by square brackets inside the `useMemo` hook are used as conditions to tell React not to update unless any of those conditions have changed.

### Running during initialization

Sometimes, when you have code that needs to run only once when the application initializes, then you might be tempted to place it inside a `useEffect` hook. However, this practice is flawed as there might be unforeseen errors, or it might bring up some cause issues during development as components are remounted after being mounted, so Effects would run twice.

There are two approaches to dealing with this. The first of which is to check whether the application has already been initialized using a variable:

```jsx
let didInit = false;

function App() {
  useEffect(() => {
    if (!didInit) {
      didInit = true;
      // ✅ Only runs once per app load
      loadDataFromLocalStorage();
      checkAuthToken();
    }
  }, []);
  // ...
}
```

The second approach is to run it during module initialization and before the app renders:

```jsx
if (typeof window !== 'undefined') { // Check if we're running in the browser.
   // ✅ Only runs once per app load
  checkAuthToken();
  loadDataFromLocalStorage();
}

function App() {
  // ...
}
```

### Subscribing to an External Store

When you need to subscribe to an external data store then it is normal to make use of the `useEffect` hook. However, react has a special hook built just for this purpose called the `useSyncExternalStore` hook.

### Fetching Data

When writing a fetch function, it is best to add a cleanup function to ensure that any stale responses are dealt with.

```jsx
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  const [page, setPage] = useState(1);
  useEffect(() => {
    let ignore = false;
    fetchResults(query, page).then(json => {
      if (!ignore) {
        setResults(json);
      }
    });
    return () => {
      ignore = true;
    };
  }, [query, page]);

  function handleNextPageClick() {
    setPage(page + 1);
  }
  // ...
}
```

## Lifecycle of Reactive Effects



---

## Source(s)

[Referencing Values with Refs | React](https://react.dev/learn/referencing-values-with-refs)
[Synchronizing with Effects | React](https://react.dev/learn/synchronizing-with-effects)
[You Might Not Need an Effect | React](https://react.dev/learn/you-might-not-need-an-effect)

---
#reactjs