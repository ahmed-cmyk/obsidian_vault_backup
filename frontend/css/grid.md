# CSS: Grid

CSS Grid Layout (or "Grid") is a two-dimensional layout system for the web. It allows you to design complex responsive web design layouts more easily and consistently across different browsers. Unlike Flexbox (which is one-dimensional), Grid can handle both rows and columns simultaneously.

---
## Grid Container and Grid Items

Grid layout consists of a **grid container** (the parent element) and **grid items** (its direct children).

To create a grid container, set the `display` property to `grid` or `inline-grid`.

```html
<div class="grid-container">
  <div class="grid-item">1</div>
  <div class="grid-item">2</div>
  <div class="grid-item">3</div>
  <div class="grid-item">4</div>
  <div class="grid-item">5</div>
  <div class="grid-item">6</div>
</div>
```

```css
.grid-container {
  display: grid; /* Makes the div a grid container */
  /* Other grid properties go here */
}

.grid-item {
  /* Properties for grid items go here */
}
```

---
## Properties for the Grid Container

### `grid-template-columns` and `grid-template-rows`

Define the columns and rows of the grid with a space-separated list of values. The values represent the track size, and the space between them represents the grid line.

*   **Fixed values**: `px`, `em`, `rem`, etc.
*   **Flexible values**: `fr` unit (a fraction of the available space).
*   `repeat()`: A function to repeat a pattern.
*   `minmax()`: Defines a size range greater than or equal to `min` and less than or equal to `max`.
*   `auto`: Automatically sizes the column/row.

```css
.grid-container {
  grid-template-columns: 100px 1fr 2fr; /* Fixed, 1 fraction, 2 fractions */
  grid-template-rows: auto 100px;

  /* Using repeat() */
  grid-template-columns: repeat(3, 1fr); /* Three equal columns */

  /* Using minmax() */
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); /* Responsive columns */
}
```

### `grid-gap` (or `gap`, `row-gap`, `column-gap`)

Sets the size of the gaps between rows and columns.

```css
.grid-container {
  grid-gap: 10px; /* 10px gap between rows and columns */
  /* or */
  row-gap: 15px;
  column-gap: 20px;
}
```

### `justify-items` and `align-items`

Aligns grid items within their grid cells.

*   `justify-items`: Aligns items along the row (inline) axis.
*   `align-items`: Aligns items along the column (block) axis.

Values: `start`, `end`, `center`, `stretch` (default).

```css
.grid-container {
  justify-items: center;
  align-items: center;
}
```

### `justify-content` and `align-content`

Aligns the grid tracks within the grid container when there is extra space.

*   `justify-content`: Aligns the grid along the row (inline) axis.
*   `align-content`: Aligns the grid along the column (block) axis.

Values: `start`, `end`, `center`, `space-between`, `space-around`, `space-evenly`, `stretch` (default).

```css
.grid-container {
  justify-content: space-around;
  align-content: center;
  height: 500px; /* Needs defined height for align-content to work */
}
```

---
## Properties for the Grid Items

### `grid-column` and `grid-row`

Specify the size and location of a grid item within the grid by referring to grid lines.

*   `grid-column-start`, `grid-column-end`
*   `grid-row-start`, `grid-row-end`

Shorthand:

*   `grid-column: start-line / end-line;`
*   `grid-row: start-line / end-line;`

Or using `span`:

*   `grid-column: span 2;` (span 2 columns)

```css
.grid-item-1 {
  grid-column-start: 1;
  grid-column-end: 3; /* Spans from line 1 to line 3 (i.e., 2 columns) */
  /* Shorthand: grid-column: 1 / 3; */
}

.grid-item-2 {
  grid-row: 2 / span 2; /* Starts at row line 2, spans 2 rows */
}
```

### `grid-area`

Assigns a name to a grid item, or specifies its size and location using line names or numbers.

*   `grid-area: row-start / column-start / row-end / column-end;`

```css
.grid-container {
  grid-template-areas: 
    "header header header"
    "nav    main   aside"
    "footer footer footer";
}

.header { grid-area: header; }
.nav    { grid-area: nav; }
.main   { grid-area: main; }
.aside  { grid-area: aside; }
.footer { grid-area: footer; }
```

### `justify-self` and `align-self`

Aligns a single grid item within its grid cell, overriding the `justify-items` and `align-items` values set on the container.

Values: `start`, `end`, `center`, `stretch` (default).

```css
.grid-item-special {
  justify-self: end;
  align-self: start;
}
```

CSS Grid is a powerful tool for creating complex, responsive, and robust layouts. It excels at defining the overall structure of a page or a major section, making it a cornerstone of modern web design.

---