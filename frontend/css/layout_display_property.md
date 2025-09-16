# CSS: Layout - The `display` Property

The `display` property is one of the most important CSS properties for controlling the layout of elements. Every HTML element has a default `display` value, but you can override it. Understanding `display` is key to mastering CSS layouts.

---
## Common `display` Values

### 1. `display: block`

*   Elements with `display: block` start on a new line and take up the full width available (stretches as far left and right as it can).
*   You can set `width`, `height`, `margin`, and `padding`.
*   Examples: `<div>`, `<p>`, `<h1>` to `<h6>`, `<ul>`, `<li>`, `<form>`, `<header>`, `<footer>`, `<section>`.

```css
div {
  display: block;
  width: 100px;
  height: 100px;
  background-color: lightblue;
  margin: 10px;
}
```

### 2. `display: inline`

*   Elements with `display: inline` do not start on a new line and only take up as much width as necessary.
*   You cannot set `width` or `height`. `margin-top` and `margin-bottom` have no effect (though `margin-left` and `margin-right` do). `padding-top` and `padding-bottom` will add space but won't affect the layout of surrounding elements.
*   Examples: `<span>`, `<a>`, `<img>`, `<strong>`, `<em>`.

```css
span {
  display: inline;
  background-color: lightgreen;
  padding: 5px;
  margin: 5px; /* Only left/right margin will visually affect layout */
}
```

### 3. `display: inline-block`

*   Elements with `display: inline-block` are like `inline` elements, but you *can* set `width` and `height`, and `margin` and `padding` on all sides will be respected.
*   They do not start on a new line.
*   Useful for creating grid-like layouts before Flexbox or Grid became widely adopted, or for elements like navigation items that need dimensions but should flow horizontally.

```css
.box {
  display: inline-block;
  width: 150px;
  height: 100px;
  background-color: lightcoral;
  margin: 10px;
}
```

### 4. `display: none`

*   Completely hides an element. The element will not take up any space on the page, and it will be as if it doesn't exist in the document flow.
*   Often used with JavaScript to show/hide elements dynamically.

```css
.hidden-element {
  display: none;
}
```

**Note**: `visibility: hidden;` also hides an element, but the element still takes up space in the layout.

### 5. `display: flex`

*   Turns an element into a flex container, enabling a flex context for all its direct children.
*   This is the foundation for Flexbox layouts, which are designed for one-dimensional layouts (either row or column).

```css
.container {
  display: flex;
  justify-content: center; /* Horizontally center children */
  align-items: center;     /* Vertically center children */
}
```

(See `flexbox.md` for more details.)

### 6. `display: grid`

*   Turns an element into a grid container, enabling a grid context for all its direct children.
*   This is the foundation for CSS Grid layouts, which are designed for two-dimensional layouts (rows and columns simultaneously).

```css
.grid-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr; /* Three equal columns */
  gap: 10px;
}
```

(See `grid.md` for more details.)

### 7. `display: table`, `display: table-cell`, etc.

*   These values make elements behave like HTML table elements. While useful in some specific cases, Flexbox and Grid are generally preferred for modern layouts.

```css
.table-container {
  display: table;
  width: 100%;
}
.table-cell {
  display: table-cell;
  vertical-align: middle;
}
```

---
## Choosing the Right `display` Value

*   Use `block` for major sections, paragraphs, and elements that should occupy their own line.
*   Use `inline` for text-level elements that should flow within a line of text.
*   Use `inline-block` when you need `inline` flow but also want to control `width`, `height`, and full `margin`/`padding`.
*   Use `flex` for one-dimensional layouts (rows or columns) where you need powerful alignment and distribution controls.
*   Use `grid` for two-dimensional layouts (rows and columns) where you need precise control over item placement and sizing in both directions.

The `display` property is a cornerstone of CSS layout, and mastering its various values is essential for building effective and responsive web designs.

---