# Layout Widgets in Flutter

Flutter provides a rich set of widgets for arranging and constraining other widgets on the screen.

## Single-Child Layout Widgets:
-   **`Container`:** A convenience widget that combines common painting, positioning, and sizing widgets. Can have `padding`, `margin`, `color`, `decoration`, `width`, `height`, `alignment`, and `child`.
-   **`Padding`:** Inserts empty space around its child.
-   **`Center`:** Centers its child within itself.
-   **`Align`:** Aligns its child within itself and optionally sizes itself based on the child's size.
-   **`Expanded` / `Flexible`:** Used within `Row`, `Column`, or `Flex` to control how a child widget fills the available space. `Expanded` forces the child to fill the available space, while `Flexible` allows the child to be flexible but not necessarily fill all space.

## Multi-Child Layout Widgets:
-   **`Row`:** Arranges its children horizontally.
-   **`Column`:** Arranges its children vertically.
-   **`Stack`:** Overlaps its children, positioning them relative to the edges of the stack. Useful for placing widgets on top of each other (e.g., text over an image).
-   **`ListView`:** A scrollable list of widgets arranged linearly. Efficient for long lists as it only renders visible items.
-   **`GridView`:** Arranges its children in a 2D scrollable grid.

## Main and Cross Axis Alignment:
-   **`Row`:** `mainAxisAlignment` (horizontal), `crossAxisAlignment` (vertical).
-   **`Column`:** `mainAxisAlignment` (vertical), `crossAxisAlignment` (horizontal).

## Understanding Constraints:
Flutter's layout system is based on "constraints go down, sizes go up, parent sets position." A parent widget passes constraints to its children, children return their sizes, and the parent then positions the children.