# React and Web Components

## What Are Web Components?

Web Components are a set of standardized browser APIs that let you create reusable, encapsulated, and composable custom HTML elements:

* **Custom Elements**: Define new HTML tags with custom behavior.
* **Shadow DOM**: Encapsulates styles and markup inside a component, preventing leakage.
* **HTML Templates & Slots**: Define reusable markup and placeholders.

Example of a plain Web Component:

```jsx
class MyElement extends HTMLElement {
  connectedCallback() {
    this.innerHTML = `<p>Hello from Web Component</p>`;
  }
}
customElements.define('my-element', MyElement);
```

And use it in HTML/JSX:

```jsx
<my-element></my-element>
```

---
## Why Use Web Components with React?

* To integrate with third-party libraries or frameworks that expose functionality as Web Components.
* To create framework-agnostic components that work with React, Vue, Angular, or vanilla JS.
* To encapsulate styles and markup at the browser level (via Shadow DOM).

---
## Challenges When Combining React & Web Components

### 1. Props & Attributes

React automatically sets DOM properties, while Web Components usually work with attributes.

Solution: Use `.setAttribute()` or explicitly define properties.

```jsx
<my-element some-attr="value"></my-element>
```

If the Web Component expects a property instead of an attribute:

```jsx
const ref = useRef();

useEffect(() => {
  if (ref.current) {
    ref.current.someProp = 'value';
  }
}, []);
```

```jsx
<my-element ref={ref}></my-element>
```

---
### 2. Events

React uses its own synthetic event system, while Web Components dispatch native DOM events.

Solution: Listen using `.addEventListener()` instead of React props.

```jsx
useEffect(() => {
  const el = ref.current;
  const handler = e => console.log(e.detail);

  el.addEventListener('custom-event', handler);
  return () => el.removeEventListener('custom-event', handler);
}, []);
```

---
## Best Practices

* Always check the Web Component’s documentation to know whether it expects props or attributes.
* Prefer `ref` and `useEffect` for interacting with the component’s API.
* Be cautious of styling conflicts: React styles won’t penetrate a Web Component’s Shadow DOM.
* Avoid using React state to control internal state of a Web Component unless it explicitly exposes an API for it.

---
## Common Mistakes to Avoid

* Assuming React props automatically map to Web Component attributes or properties.
* Trying to handle `onEvent` props (React-style) for native Web Component events.
* Ignoring the Shadow DOM — styles and selectors don’t work the same way.

---

In summary, Web Components can work well with React when you explicitly handle their differences in lifecycle, attributes/properties, and events. Use `ref` and native DOM APIs when necessary.

---

#reactjs