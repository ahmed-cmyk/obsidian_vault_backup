# CSS: Flexbox

Flexbox (the Flexible Box Module) is a one-dimensional layout method for arranging items in rows or columns. It provides a powerful and efficient way to distribute space among items in a container and align them, even when their size is unknown or dynamic.

---
## Flex Container and Flex Items

Flexbox layout consists of a **flex container** (the parent element) and **flex items** (its direct children).

To create a flex container, set the `display` property to `flex` or `inline-flex`.

```html
<div class="flex-container">
  <div class="flex-item">1</div>
  <div class="flex-item">2</div>
  <div class="flex-item">3</div>
</div>
```

```css
.flex-container {
  display: flex; /* Makes the div a flex container */
  /* Other flex properties go here */
}

.flex-item {
  /* Properties for flex items go here */
}
```

---
## Properties for the Flex Container

### `flex-direction`

Defines the direction flex items are placed in the flex container.

*   `row` (default): Left to right in `ltr`; right to left in `rtl`.
*   `row-reverse`: Right to left in `ltr`; left to right in `rtl`.
*   `column`: Same as `row` but top to bottom.
*   `column-reverse`: Same as `row-reverse` but bottom to top.

```css
.flex-container {
  flex-direction: column;
}
```

### `flex-wrap`

Specifies whether flex items should wrap or not.

*   `nowrap` (default): All flex items will be on one line.
*   `wrap`: Flex items will wrap onto multiple lines, from top to bottom.
*   `wrap-reverse`: Flex items will wrap onto multiple lines, from bottom to top.

```css
.flex-container {
  flex-wrap: wrap;
}
```

### `flex-flow` (Shorthand for `flex-direction` and `flex-wrap`)

```css
.flex-container {
  flex-flow: row wrap; /* Equivalent to flex-direction: row; flex-wrap: wrap; */
}
```

### `justify-content`

Aligns flex items along the main axis (horizontal for `row`, vertical for `column`).

*   `flex-start` (default): Items are packed toward the start of the flex-direction.
*   `flex-end`: Items are packed toward the end of the flex-direction.
*   `center`: Items are centered along the line.
*   `space-between`: Items are evenly distributed in the line; first item is at the start, last item at the end.
*   `space-around`: Items are evenly distributed in the line with equal space around them.
*   `space-evenly`: Items are distributed so that the spacing between any two items (and the space to the edges) is equal.

```css
.flex-container {
  justify-content: space-between;
}
```

### `align-items`

Aligns flex items along the cross axis (vertical for `row`, horizontal for `column`).

*   `stretch` (default): Items are stretched to fit the container.
*   `flex-start`: Items are placed at the start of the cross axis.
*   `flex-end`: Items are placed at the end of the cross axis.
*   `center`: Items are centered on the cross axis.
*   `baseline`: Items are aligned such as their baselines align.

```css
.flex-container {
  align-items: center;
}
```

### `align-content`

Aligns a flex container's lines within the flex container when there is extra space in the cross-axis. This property has no effect when `flex-wrap` is `nowrap`.

*   `flex-start`, `flex-end`, `center`, `space-between`, `space-around`, `stretch` (default).

```css
.flex-container {
  flex-wrap: wrap;
  align-content: space-around;
}
```

---
## Properties for the Flex Items

### `order`

Specifies the order of a flexible item relative to the rest of the flex items inside the same container. Default is `0`.

```css
.item-1 { order: 2; }
.item-2 { order: 1; }
```

### `flex-grow`

Specifies how much a flex item will grow relative to the rest of the flex items. Default is `0` (no growth).

```css
.flex-item {
  flex-grow: 1; /* All items grow equally */
}
.special-item {
  flex-grow: 2; /* This item grows twice as much */
}
```

### `flex-shrink`

Specifies how much a flex item will shrink relative to the rest of the flex items. Default is `1` (all items shrink equally).

```css
.flex-item {
  flex-shrink: 1;
}
.no-shrink-item {
  flex-shrink: 0; /* This item will not shrink */
}
```

### `flex-basis`

Specifies the initial size of a flex item before any growing or shrinking. It can be a length (`px`, `em`, `%`) or `auto` (default).

```css
.flex-item {
  flex-basis: 100px;
}
```

### `flex` (Shorthand for `flex-grow`, `flex-shrink`, and `flex-basis`)

```css
.flex-item {
  flex: 1 1 auto; /* grow shrink basis */
}

.flex-item-fixed {
  flex: 0 0 200px; /* Don't grow, don't shrink, fixed width of 200px */
}

.flex-item-grow {
  flex: 1; /* Equivalent to flex: 1 1 0%; */
}
```

### `align-self`

Overrides the `align-items` value for a single flex item.

*   `auto` (default)
*   `flex-start`, `flex-end`, `center`, `baseline`, `stretch`

```css
.special-item {
  align-self: flex-end;
}
```

Flexbox is incredibly powerful for creating responsive and dynamic layouts, especially for components that need to align and distribute space along a single axis.

---