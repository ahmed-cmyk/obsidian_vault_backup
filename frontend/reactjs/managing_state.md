# Managing State

When you want to share state between two or more components, you should place it in the closest common parent, and then pass it down to them via props. This practice is referred to as **lifting state up**.

## Keys

When you have two or more of the same component, the state is not shared between them. The reason why is because React looks at the entire tree and knows what state belongs to what node on the tree based on their *order within the parent*.

> **Note**: This can cause issues when replacing a component as react would see that the new component is in the same position, hence, the state of the previously destroyed component would carry on.

To prevent such issues, it is best to use `keys`.

```jsx
if (value === true) {
	<Counter key="first" />
} else {
	<Counter key="second" />
}
```

> **Note**: Specifying a key tells React to use the keys instead of their order to determine their position.

## Manage State Using Reducers

**The steps to manage your state using reducers includes**:
1. Create dispatch actions in your event handlers.
2. Write a reducer function to store all of your state logic.
3. Use the reducer from your component

Managing state using reducers is slightly different from directly setting state. Instead of telling React “what to do” by setting state, you specify “what the user just did” by dispatching actions from your event handlers.

```jsx
handleAdd(value) {
	dispatch({
		type: 'added',
		value: value,
	});
}
```

> **Note**: The object you pass to dispatch is called an *action*.

> **Add. Note**: An action object can have any shape. By convention, it is common to give it a string `type` that describes what happened, and pass any additional information in other fields.

Next, you want to write a `reducer` function. A reducer function is where you will put your *state logic*. It takes two arguments, the current state and the action object, and it returns the next state.

```jsx
function yourReducer(state, action) {
  // return next state for React to set
}
```

To execute code based on the type of action submitted, you could either use `if-else` statements or rely on `switch-statements`.

> **Note**: It is convention to use `switch-statements` inside reducers as it tends to be easier to read.

```jsx
function yourReducer(state, action) {
	switch(action.type) {
		case 'added':
			// code to run
			return state
	}
}
```

Finally, you want to hook up your reducer to the component. Import the `useReducer` hook from React:

```jsx
import { useReducer } from 'react';
```

Then, you can replace `useState`:

```jsx
const [tasks, dispatch] = useReducer(tasksReducer, intiialTasks);
```

**The `useReducer` hook takes two arguments**:
1. A reducer function
2. An initial state

**And it returns**:
1. A stateful value
2. A dispatch function (to "dispatch" user actions to the reducer)

> **Imp**: Reducers should not send requests, schedule timeouts, or perform any side effects (operations that impact things outside the component). They should update objects and arrays without mutations.

### Passing Data Deeply with Context

Context allows the parent component to make information to any component below it without explicitly passing it through props.

**There are three steps to using a context**:
1. Create a context
2. Use that context from the component that needs the data
3. Provide that context from the component that specifies the data

Before using context you need to import `createContext`:

```jsx
import { createContext } from 'react'; 

export const LevelContext = createContext(1);
```

> **Note**: The only argument for `createContext` is the default value. You can pass any type of value (even an object).

Next, go to the component that needs the data and import the context that you created, and also the `useContext` hook from React.

```jsx
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';
```

To use the context, just call the `useContext` hook inside the component that you wish to share context with, and then pass the imported context as an argument into your `useContext` call. Store the returned value into a variable.

```jsx
export default function Heading() {
	const level = useContext(LevelContext);
}
```

To provide context to the children elements or nodes, you have to wrap them inside a context provider.

```jsx
<section>
	<LevelContext.Provider value={level}>
		{children}
	</LevelContext.Provider>
</section>
```

> **Note**: You can combine `reducers` and `context` to provide dispatching throughout the app without having to pass props down through hundreds of components.

---

## Source(s)

[Reacting to Input with State | React](https://react.dev/learn/reacting-to-input-with-state)
[Sharing State Between Components | React](https://react.dev/learn/sharing-state-between-components)
[Passing Data Deeply with Context | React](https://react.dev/learn/passing-data-deeply-with-context)
[Scaling Up with Reducer and Context | React](https://react.dev/learn/scaling-up-with-reducer-and-context)

---
#reactjs