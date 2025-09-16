# Handling User Input

This note covers various ways to handle user input in React Native, including touch events, text input, and gestures.

## Touch Events

- **`Pressable`:** A core component to detect various types of presses.
- **`TouchableOpacity`:** A wrapper for making views respond properly to touches. On press, the opacity of the wrapped view is decreased.
- **`TouchableHighlight`:** A wrapper for making views respond properly to touches. On press, the wrapped view is darkened.
- **`TouchableWithoutFeedback`:** Renders its children without any feedback when touched.

## Text Input

- **`TextInput` Component:** Used for allowing users to input text.
- **Props:** `onChangeText`, `value`, `placeholder`, `keyboardType`, `secureTextEntry`, etc.

## Gestures

- **`PanResponder`:** Provides a highly configurable system for tracking touches and gestures.
- **Third-Party Libraries:** Libraries like `react-native-gesture-handler` offer more advanced and declarative gesture handling.

## Example (TextInput and Button)

```jsx
import React, { useState } from 'react';
import { Button, Text, TextInput, View, StyleSheet } from 'react-native';

const UserInputExample = () => {
  const [name, setName] = useState('');
  const [submittedName, setSubmittedName] = useState('');

  const handleSubmit = () => {
    setSubmittedName(name);
    setName('');
  };

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.input}
        placeholder="Enter your name"
        onChangeText={setName}
        value={name}
      />
      <Button title="Submit" onPress={handleSubmit} />
      {submittedName ? <Text style={styles.displayText}>Hello, {submittedName}!</Text> : null}
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    width: '100%',
    marginBottom: 20,
    paddingHorizontal: 10,
  },
  displayText: {
    marginTop: 20,
    fontSize: 18,
  },
});

export default UserInputExample;
```