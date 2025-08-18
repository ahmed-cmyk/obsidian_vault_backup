# Rendering to the DOM

In your React project, there is a usually a HTML file that contains the following:

```html
<div id="root"></div>
```

> **Note**: Applications built with just React usually have a single root DOM node. If you are integrating React into an existing app, you may have as many isolated root DOM nodes as you like.

To render a React element, first pass the DOM element to `ReactDOM.createRoot()`, then pass the React element to `root.render()`.

**Example**:

```jsx
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
const element = <h1>Hello, world</h1>;
root.render(element);
```

> **Imp.**: React elements are immutable. Once you create an element, you can’t change its children or attributes.

**Source**: [Rendering to the DOM | React](https://legacy.reactjs.org/docs/rendering-elements.html)

---
#reactjs