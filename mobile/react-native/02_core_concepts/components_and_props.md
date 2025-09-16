# Components and Props

This note explains the core concepts of React Native components, how to create them, and how to pass data between them using props.

## Components

- **Functional Components:** Simple JavaScript functions that return JSX.
- **Class Components:** ES6 classes that extend `React.Component` and have lifecycle methods.

## Props (Properties)

- **Definition:** Props are arguments passed into React components.
- **Read-Only:** Props are read-only; a component must never modify its own props.
- **Usage:** Used to pass data from a parent component to a child component.

## Example

```jsx
import React from 'react';
import { Text, View } from 'react-native';

const Welcome = (props) => {
  return (
    <View>
      <Text>Hello, {props.name}!</Text>
    </View>
  );
};

const App = () => {
  return (
    <View>
      <Welcome name="Ahmed" />
      <Welcome name="Gemini" />
    </View>
  );
};

export default App;
```