# Component Patterns & Data Flow

## Presentational & Container Components (Smart & Dumb)

### Why?

Separates *concerns*:

* Presentational: How it **looks**
* Container: How it **works**

### Presentational Component

* Focus: UI only
* Receives props from parent
* No business logic or state (except minor UI state)
* Reusable & easy to test

```jsx
function UserCard({ name, onClick }) {
  return (
    <div>
      <h2>{name}</h2>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}
```

### Container Component

* Handles data fetching, state, and logic
* Uses presentational components to render UI

```jsx
function UserCardContainer() {
  const [user, setUser] = React.useState({ name: 'Alice' });

  const handleClick = () => {
    alert(`Hello, ${user.name}`);
  };

  return <UserCard name={user.name} onClick={handleClick} />;
}
```

**Common mistakes to avoid:**

* Mixing business logic in presentational components.
* Making containers too large and unmanageable — break them down if needed.
* Over-abstraction: not every component needs this split.

---
## Higher-Order Components (HOC)

A **HOC** is a function that **wraps a component** and adds new props/behavior.

```jsx
function withLogger(WrappedComponent) {
  return function Enhanced(props) {
    console.log(`Rendering ${WrappedComponent.name}`);
    return <WrappedComponent {...props} />;
  };
}
```

### When to use HOC?

* Add cross-cutting concerns like:
  * Authentication
  * Logging
  * Permissions
  * Caching

**Common mistakes to avoid:**

* Naming collisions: props passed down by HOC may conflict with existing props.
* Nesting too many HOCs leads to “wrapper hell”.
* Forgetting to forward refs if needed.

---
## Render Props

A **pattern** where you pass a **function** as a prop and that function returns JSX.

```jsx
function DataFetcher({ render }) {
  const [data, setData] = React.useState(null);

  React.useEffect(() => {
    fetch('/api').then((res) => res.json()).then(setData);
  }, []);

  return render(data);
}

<DataFetcher render={(data) =>
  <div>{data ? data.value : 'Loading...'}</div>
} />
```

### Why use it?

* Allows logic reuse while letting the consumer define the rendering.

**Common mistakes to avoid:**

* Inline anonymous functions can hurt performance because a new function is created on every render.
* Overcomplicating the render prop instead of using hooks for logic reuse.

---
## Compound Components

A group of related components that share state but can be composed flexibly.

```jsx
function Tabs({ children }) {
  const [active, setActive] = React.useState(0);
  return React.Children.map(children, (child, index) =>
    React.cloneElement(child, { active, setActive, index })
  );
}

function Tab({ active, setActive, index, children }) {
  return (
    <button
      style={{ fontWeight: active === index ? 'bold' : 'normal' }}
      onClick={() => setActive(index)}
    >
      {children}
    </button>
  );
}

<Tabs>
  <Tab>Tab 1</Tab>
  <Tab>Tab 2</Tab>
</Tabs>
```

**Common mistakes to avoid:**

* Tightly coupling internal state to a specific markup structure.
* Not exposing enough control to the consumer when needed.

---

## Prop Drilling & How to Avoid It
### Prop Drilling

Passing props through multiple layers of components unnecessarily.

```jsx
function App() {
  const user = { name: 'Alice' };
  return <Parent user={user} />;
}
```

**How to avoid:**

* React Context API
* State management libraries (Redux, Zustand)
* Lift state closer to where it’s needed

**Common mistakes to avoid:**

* Overusing Context — it’s not always the answer; sometimes just move the state.
* Storing unrelated state in Context just to avoid drilling.

---
## Controlled vs Uncontrolled Components

This describes how form inputs are managed.

---
### Controlled Component

* React manages the state.
* Value comes from state, updated via `onChange`.
* React is the source of truth.

```jsx
function MyForm() {
  const [value, setValue] = React.useState('');

  const handleChange = (e) => setValue(e.target.value);

  return (
    <form>
      <input value={value} onChange={handleChange} />
      <p>Value: {value}</p>
    </form>
  );
}
```

### Advantages

* Predictable, easier validation.
* Centralized state.
* Resets are simple.

---

### Uncontrolled Component

* DOM keeps track of its own value.
* You only read the value when you need it (via a ref).

```jsx
function MyForm() {
  const inputRef = React.useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    alert(`Value: ${inputRef.current.value}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Advantages

* Simpler for basic forms.
* Slightly better performance — fewer renders.
* Useful with 3rd-party libraries.

---
### Comparison Table

| Feature | Controlled | Uncontrolled |
|---|---|---|
| State managed by | React | DOM |
| Updates via | `onChange` + `setState` | `ref.current.value` | 
| Validation | Easier | Harder |
| Reset | Simple | Manual |
| Best for | Dynamic forms, validation | Simple, static forms |

---

**Common mistakes to avoid:**

* Mixing controlled and uncontrolled on the same input — it causes React warnings.
* Forgetting to initialize state for controlled components.
* Not using `defaultValue` or `defaultChecked` with uncontrolled components.

---

## Summary of Common Mistakes Across Patterns

* Overengineering small apps with too many patterns.
* Ignoring readability by nesting too many HOCs or render props.
* Forgetting to pass down `props` or `ref` properly.
* Overusing Context where simpler state lifting is enough.
* Forgetting accessibility when creating custom components (e.g., Tabs, forms).
* Mixing controlled & uncontrolled patterns in forms.
* Creating unnecessary re-renders by defining functions inside `render`.

---

#reactjs