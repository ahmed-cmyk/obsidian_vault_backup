# Widgets Overview in Flutter

Widgets are the fundamental building blocks of Flutter applications. They describe the UI and its behavior.

## Types of Widgets:
1.  **StatelessWidget:**
    -   Does not require mutable state.
    -   Its properties are immutable.
    -   Examples: `Text`, `Icon`, `Image`.
    -   `build` method is called once.

2.  **StatefulWidget:**
    -   Has mutable state that can change over its lifetime.
    -   Requires a `State` object to manage its mutable state.
    -   Examples: `Checkbox`, `Slider`, `TextField`.
    -   `createState` method returns a `State` object.
    -   `setState()` is used to notify the framework that the internal state of a `State` object has changed, which causes the UI to rebuild.

## Widget Tree:
Flutter UIs are built as a tree of widgets. Each widget nests inside another, forming a hierarchy.

## Key Widget Properties:
-   `key`: Used to control how a widget replaces another widget in the tree.
-   `child`/`children`: For single-child or multi-child widgets, respectively.
-   `padding`, `margin`, `alignment`: Common properties for layout and positioning.