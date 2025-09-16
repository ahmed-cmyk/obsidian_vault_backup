# State and Lifecycle

This note covers the concepts of state in React Native components and the lifecycle methods available for managing component behavior over time.

## State

- **Definition:** State is data that changes over time within a component. It is mutable and managed within the component itself.
- **`useState` Hook (Functional Components):** Used to add state to functional components.
- **`this.state` (Class Components):** Used to manage state in class components.

## Lifecycle Methods (Class Components)

- **Mounting:** `constructor()`, `static getDerivedStateFromProps()`, `render()`, `componentDidMount()`
- **Updating:** `static getDerivedStateFromProps()`, `shouldComponentUpdate()`, `render()`, `getSnapshotBeforeUpdate()`, `componentDidUpdate()`
- **Unmounting:** `componentWillUnmount()`

## `useEffect` Hook (Functional Components)

- **Purpose:** Used to handle side effects (data fetching, subscriptions, manually changing the DOM) in functional components.
- **Analogue to Lifecycle Methods:** Can be used to mimic `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

## Example (useState)

```jsx
import React, { useState } from 'react';
import { Button, Text, View } from 'react-native';

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="Increment" onPress={() => setCount(count + 1)} />
    </View>
  );
};

export default Counter;
```