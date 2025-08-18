# React with TypeScript

TypeScript adds **static typing** to JavaScript, which helps catch bugs earlier, improve IDE support, and make your React code more maintainable and self-documenting. Using React with TypeScript involves defining types for props, state, hooks, and components.

---

## Why Use TypeScript with React?

* Detects type errors at compile time rather than at runtime.
* Improves code readability and documentation.
* Enables better auto-completion and navigation in editors.
* Helps teams understand component contracts clearly.

---
## Setting Up

You can create a new React + TypeScript project with:

```bash
npx create-react-app my-app --template typescript
```

or if using Vite:

```bash
npm create vite@latest my-app --template react-ts
```

For an existing project:

```bash
npm install --save-dev typescript @types/react @types/react-dom
```

---
## Typing Functional Components

```jsx
type Props = {
  name: string;
  age?: number; // optional prop
};

const Greeting: React.FC<Props> = ({ name, age }) => {
  return (
    <div>
      Hello, {name}! {age && `You are ${age} years old.`}
    </div>
  );
};
```

You can also write components without `React.FC`:

```jsx
const Greeting = ({ name }: Props) => <div>Hello, {name}</div>;
```

---
## Typing Hooks

### useState

```jsx
const [count, setCount] = React.useState<number>(0);
```

### useRef

```jsx
const inputRef = React.useRef<HTMLInputElement>(null);
```

### useReducer

```jsx
type State = { count: number };
type Action = { type: 'increment' | 'decrement' };

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
  }
};

const [state, dispatch] = React.useReducer(reducer, { count: 0 });
```

---
## Typing Events

```jsx
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  console.log(e.target.value);
};

const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
};
```

---

## Typing Children

```jsx
type PropsWithChildren = {
  children: React.ReactNode;
};

const Wrapper: React.FC<PropsWithChildren> = ({ children }) => {
  return <div>{children}</div>;
};
```

---
## Typing Context

```jsx
type Theme = 'light' | 'dark';

const ThemeContext = React.createContext<Theme>('light');
```

---
## Common Mistakes to Avoid

🚫 Using `any` everywhere — defeats the purpose of TypeScript.
🚫 Forgetting to type default state in `useState` (defaults to `undefined`).
🚫 Overcomplicating types — keep them simple and clear.
🚫 Not installing `@types` packages for third-party libraries.

---
## Best Practices

✅ Use `strict` mode in `tsconfig.json`.
✅ Prefer specific types over `any`.
✅ Type props and state explicitly.
✅ Leverage TypeScript with IDE tooling for better productivity.
✅ Incrementally adopt TypeScript in existing projects if needed.

---

#reactjs