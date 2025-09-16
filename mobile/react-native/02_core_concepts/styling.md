# Styling in React Native

This note explains how to style your React Native components using `StyleSheet`, inline styles, and other styling techniques.

## StyleSheet API

- **Purpose:** Provides an abstraction similar to CSS stylesheets.
- **Benefits:** Performance optimizations, type checking, and better organization.

## Inline Styles

- **Usage:** Directly apply style objects to components.
- **Considerations:** Can lead to code duplication and reduced readability for complex styles.

## Styling Components

- **`View`:** The most fundamental component for building UI, similar to a `div`.
- **`Text`:** For displaying text.
- **`Image`:** For displaying images.

## Flexbox

- **Layout System:** React Native uses Flexbox for all layout purposes.
- **Concepts:** `flexDirection`, `justifyContent`, `alignItems`, `flex`, etc.

## Example

```jsx
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

const App = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello, React Native!</Text>
      <Text style={styles.subtitle}>Styling is fun.</Text>
    </View>
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
    textAlign: 'center',
    margin: 10,
    color: '#333333',
  },
  subtitle: {
    fontSize: 18,
    textAlign: 'center',
    color: '#666666',
  },
});

export default App;
```