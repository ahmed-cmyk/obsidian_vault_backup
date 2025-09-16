# CSS: Selectors

CSS selectors are patterns used to select the HTML elements you want to style. They are the fundamental building blocks of CSS, allowing you to target specific elements or groups of elements on a web page.

---
## 1. Simple Selectors

### Element Selector

Selects HTML elements based on their tag name.

```css
p {
  color: blue;
  text-align: center;
}
```

### ID Selector

Selects an HTML element with a specific `id` attribute. IDs must be unique within an HTML document.

```css
#header {
  background-color: black;
  color: white;
}
```

```html
<div id="header">...</div>
```

### Class Selector

Selects HTML elements with a specific `class` attribute. Classes can be used on multiple elements.

```css
.center {
  text-align: center;
  color: red;
}
```

```html
<h1 class="center">This is a heading</h1>
<p class="center">This is a paragraph.</p>
```

### Universal Selector

Selects all HTML elements on the page.

```css
* {
  margin: 0;
  padding: 0;
}
```

---
## 2. Combinator Selectors

Combinators explain the relationship between the selectors.

### Descendant Selector (space)

Selects all elements that are descendants of a specified element.

```css
div p {
  background-color: yellow;
}
```

```html
<div>
  <p>Paragraph 1 in the div.</p>
  <span><p>Paragraph 2 in the span in the div.</p></span>
</div>
<p>Paragraph 3. Not in a div.</p>
```

### Child Selector (`>`)

Selects all elements that are direct children of a specified element.

```css
div > p {
  background-color: yellow;
}
```

```html
<div>
  <p>Paragraph 1 in the div.</p> <!-- Selected -->
  <span><p>Paragraph 2 in the span in the div.</p></span> <!-- Not selected -->
</div>
<p>Paragraph 3. Not in a div.</p>
```

### Adjacent Sibling Selector (`+`)

Selects an element that is immediately preceded by a specified element.

```css
div + p {
  background-color: yellow;
}
```

```html
<div>
  <p>Paragraph 1.</p>
</div>
<p>Paragraph 2.</p> <!-- Selected -->
<span><p>Paragraph 3.</p></span>
<p>Paragraph 4.</p> <!-- Not selected -->
```

### General Sibling Selector (`~`)

Selects all elements that are siblings of a specified element.

```css
div ~ p {
  background-color: yellow;
}
```

```html
<div>
  <p>Paragraph 1.</p>
</div>
<p>Paragraph 2.</p> <!-- Selected -->
<span><p>Paragraph 3.</p></span>
<p>Paragraph 4.</p> <!-- Selected -->
```

---
## 3. Pseudo-classes

Used to define a special state of an element. For example, it can be used to style an element when a user mouses over it, or to style visited/unvisited links.

```css
a:hover {
  color: hotpink;
}

p:first-child {
  font-weight: bold;
}

input:focus {
  border: 2px solid blue;
}
```

Common pseudo-classes:

*   `:hover`: When the user mouses over an element.
*   `:active`: When an element is being activated by the user (e.g., clicked).
*   `:focus`: When an element has focus (e.g., an input field).
*   `:link`: Unvisited links.
*   `:visited`: Visited links.
*   `:first-child`: The first child of its parent.
*   `:last-child`: The last child of its parent.
*   `:nth-child(n)`: Selects elements based on their position among siblings.

---
## 4. Pseudo-elements

Used to style a specified part of an element.

```css
p::first-line {
  color: #ff0000;
  font-variant: small-caps;
}

p::before {
  content: "Read this: ";
  color: green;
}
```

Common pseudo-elements:

*   `::first-line`: The first line of a text.
*   `::first-letter`: The first letter of a text.
*   `::before`: Inserts content before the content of an element.
*   `::after`: Inserts content after the content of an element.
*   `::selection`: The portion of an element that is highlighted by the user.

---
## 5. Attribute Selectors

Selects HTML elements based on their attributes or attribute values.

```css
[target] {
  border: 1px solid #ddd;
}

[target="_blank"] {
  background-color: yellow;
}

/* Selects all <a> elements whose href attribute value contains "example" */
a[href*="example"] {
  color: orange;
}
```

By combining these selectors, you can precisely target any element or group of elements on your web page to apply specific styles.

---