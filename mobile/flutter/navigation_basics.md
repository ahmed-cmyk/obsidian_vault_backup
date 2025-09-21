# Basic Navigation in Flutter

Navigation in Flutter involves moving between different screens (routes) in your application.

## Key Concepts:
-   **`Navigator`:** A widget that manages a stack of `Route` objects. It's like a stack data structure where routes are pushed and popped.
-   **`Route`:** An abstraction for a screen or page in your app. `MaterialPageRoute` is a common implementation for Material Design apps.

## Navigating to a New Screen:
To push a new route onto the stack:
```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => NewScreen()),
);
```
-   `context`: The `BuildContext` of the widget from which you are navigating.
-   `MaterialPageRoute`: Defines the transition animation and layout for the new screen.
-   `NewScreen()`: The widget for the destination screen.

## Returning from a Screen:
To pop the current route off the stack:
```dart
Navigator.pop(context);
```

## Navigating with Named Routes:
For more complex apps, named routes provide a cleaner way to manage navigation.

1.  **Define Routes in `MaterialApp`:**
    ```dart
    MaterialApp(
      title: 'My App',
      initialRoute: '/',
      routes: {
        '/': (context) => HomeScreen(),
        '/details': (context) => DetailScreen(),
      },
    );
    ```

2.  **Navigate to a Named Route:**
    ```dart
    Navigator.pushNamed(context, '/details');
    ```

## Passing Data Between Screens:
-   **Constructor Arguments:** For `MaterialPageRoute`, pass data directly through the constructor of the destination widget.
-   **`arguments` property:** For named routes, use the `arguments` property of `pushNamed`. Access it in the destination screen using `ModalRoute.of(context)!.settings.arguments`.