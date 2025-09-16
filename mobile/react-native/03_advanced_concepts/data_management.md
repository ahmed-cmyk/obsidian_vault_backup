# Data Management

This note explores various strategies and libraries for managing data within React Native applications, from local state to global state and persistent storage.

## Local State Management

- **`useState` and `useReducer` Hooks:** For managing state within individual components or small component trees.
- **Context API:** For sharing state across a component tree without prop drilling.

## Global State Management

- **Redux:** A predictable state container for JavaScript apps. Popular for complex applications.
  - **Redux Toolkit:** The official, opinionated, batteries-included toolset for efficient Redux development.
- **MobX:** Simple, scalable state management.
- **Zustand, Jotai, Recoil:** Lighter-weight and often simpler alternatives to Redux for global state.

## Persistent Storage

- **`AsyncStorage`:** A simple, unencrypted, asynchronous, persistent, key-value storage system for React Native.
- **Realm, SQLite:** For more complex local database needs.
- **Redux Persist:** For persisting Redux store to `AsyncStorage` or other storage engines.

## Data Fetching

- **`fetch` API / `XMLHttpRequest`:** Built-in browser APIs for making network requests.
- **Axios:** A popular promise-based HTTP client for the browser and Node.js.
- **React Query / SWR:** Libraries for managing, caching, and synchronizing asynchronous data.

## Example (Context API)

```jsx
import React, { createContext, useState, useContext } from 'react';
import { Button, Text, View, StyleSheet } from 'react-native';

// 1. Create a Context
const CountContext = createContext();

// 2. Create a Provider Component
const CountProvider = ({ children }) => {
  const [count, setCount] = useState(0);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  return (
    <CountContext.Provider value={{ count, increment, decrement }}>
      {children}
    </CountContext.Provider>
  );
};

// 3. Create a Consumer Component (or use useContext hook)
const CounterDisplay = () => {
  const { count, increment, decrement } = useContext(CountContext);

  return (
    <View style={styles.counterContainer}>
      <Text style={styles.countText}>Count: {count}</Text>
      <Button title="Increment" onPress={increment} />
      <Button title="Decrement" onPress={decrement} />
    </View>
  );
};

const App = () => {
  return (
    <CountProvider>
      <View style={styles.container}>
        <Text style={styles.title}>Context API Example</Text>
        <CounterDisplay />
      </View>
    </CountProvider>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  title: {
    fontSize: 24,
    marginBottom: 20,
  },
  counterContainer: {
    alignItems: 'center',
  },
  countText: {
    fontSize: 36,
    marginBottom: 20,
  },
});

export default App;
```