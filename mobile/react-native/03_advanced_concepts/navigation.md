# Navigation

This note discusses different strategies and libraries for implementing navigation in React Native applications, allowing users to move between different screens.

## Navigation Libraries

- **React Navigation:** The most popular and widely used navigation solution for React Native. It provides a complete navigation solution with customizable navigators (Stack, Tab, Drawer).
- **Native Stack Navigator:** Optimized for performance, using native navigation primitives.
- **Bottom Tab Navigator:** For tab-based navigation.
- **Drawer Navigator:** For a drawer (sidebar) menu.

## Key Concepts

- **Navigators:** Components that manage the navigation state and present screens.
- **Screens:** Components that represent individual views in your application.
- **Navigation Props:** `navigation` object passed to screens, containing methods like `navigate`, `push`, `goBack`, `setOptions`.

## Example (React Navigation Stack)

```jsx
import * as React from 'react';
import { Button, View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}

function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go back to Home"
        onPress={() => navigation.goBack()}
      />
    </View>
  );
}

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```