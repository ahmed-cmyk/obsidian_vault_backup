# Higher Order Components (HOCs) and Render Props

## What is a Higher Order Component (HOC)?

A **Higher Order Component (HOC)** is a function that takes a component and returns a new component with enhanced behavior.

It’s a pattern for **reusing component logic** without repeating code.

**Definition:**

> An HOC is a function:
> `(WrappedComponent) => NewComponent`

---
## Why Use HOCs?

When multiple components need to share common logic (like authentication, subscriptions, analytics), you can wrap them in an HOC instead of duplicating code.

---
## Example of an HOC

```jsx
function withLogger(WrappedComponent) {
  return function EnhancedComponent(props) {
    console.log("Rendering", WrappedComponent.name);
    return <WrappedComponent {...props} />;
  };
}

// Usage:
function MyComponent() {
  return <div>Hello</div>;
}

const MyComponentWithLogger = withLogger(MyComponent);
```

Here:

* `MyComponentWithLogger` logs whenever it renders, and then renders `MyComponent`.

---
## Common Use Cases

* Authentication checks (`withAuth`)
* Logging, analytics, error boundaries
* Fetching and injecting data
* Enhancing components with props or context

---
## Caveats of HOCs

* Prop collisions: Be careful not to overwrite props when passing them down.
* Debugging: Can make stack traces harder to follow.
* Wrapper hell: Chaining many HOCs can lead to deeply nested wrappers.

---
## What is Render Props?

**Render Props** is another pattern for sharing logic where a component accepts a function (render prop) as a prop, and calls it to render UI.

**Definition:**

> A component with a prop that is a function returning JSX.

---
## Example of Render Props

```jsx
class MouseTracker extends React.Component {
  state = { x: 0, y: 0 };

  handleMouseMove = event => {
    this.setState({ x: event.clientX, y: event.clientY });
  };

  render() {
    return (
      <div style={{ height: "100vh" }} onMouseMove={this.handleMouseMove}>
        {this.props.render(this.state)}
      </div>
    );
  }
}

// Usage:
<MouseTracker render={({ x, y }) => (
  <h1>Mouse is at ({x}, {y})</h1>
)} />
```

Here:

* The `MouseTracker` component provides `x` and `y` through the `render` prop.

---
## Common Use Cases

* Animation state
* Mouse or window dimensions
* Fetching data and rendering conditionally
* Forms and validations

---
## HOCs vs Render Props

| Feature | HOCs | Render Props |
|---|---|---|
| How it works | Wraps a component | Uses a render function |
| Props passed | Injected automatically | Explicit in render prop |
| Nesting | Can lead to wrapper hell | Can lead to prop drilling |
| Clarity | Hides implementation | More explicit & flexible |

---
## Common Mistakes

* In HOCs: Forgetting to forward refs if needed.
* In HOCs: Overwriting or omitting props.
* In Render Props: Nesting too deeply (can become hard to read).
* Both: Over-engineering when simple composition or hooks might suffice.

---

#reactjs