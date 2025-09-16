# CSS: Pseudo-classes and Pseudo-elements

CSS pseudo-classes and pseudo-elements are keywords added to selectors that allow you to style elements based on their state, position, or to style specific parts of an element without adding extra HTML classes or IDs.

---
## 1. Pseudo-classes (`:`) 

A pseudo-class is used to define a special state of an element. For example, it can be used to:

*   Style an element when a user mouses over it.
*   Style visited/unvisited links.
*   Style an element when it gets focus.
*   Select elements based on their position in the DOM tree.

### Common Pseudo-classes:

#### User Action Pseudo-classes

*   `:hover`: Selects an element when the user mouses over it.
*   `:active`: Selects an element when it is being activated by the user (e.g., clicked).
*   `:focus`: Selects an element when it has received focus (e.g., an input field).
*   `:visited`: Selects links that have already been visited.
*   `:link`: Selects unvisited links.

```css
a:link {
  color: blue;
}
a:visited {
  color: purple;
}
a:hover {
  color: red;
  text-decoration: underline;
}
a:active {
  color: yellow;
}

input:focus {
  background-color: lightyellow;
  border: 2px solid blue;
}
```

#### Structural Pseudo-classes

These select elements based on their position in the document tree.

*   `:first-child`: Selects the first child of its parent.
*   `:last-child`: Selects the last child of its parent.
*   `:nth-child(n)`: Selects elements based on their position among siblings. `n` can be a number, `odd`, `even`, or a formula (`an+b`).
*   `:nth-last-child(n)`: Similar to `nth-child`, but counts from the last child.
*   `:only-child`: Selects an element that is the only child of its parent.
*   `:empty`: Selects an element that has no children (including text nodes).

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>

<p>First paragraph.</p>
<p>Second paragraph.</p>
```

```css
li:first-child {
  color: green;
}

li:last-child {
  color: red;
}

li:nth-child(odd) {
  background-color: #f0f0f0;
}

p:only-child {
  font-style: italic;
}
```

#### Other Useful Pseudo-classes

*   `:not(selector)`: Selects elements that do not match the specified selector.
*   `:checked`: Selects checked radio buttons or checkboxes.
*   `:disabled`, `:enabled`: Selects disabled or enabled form elements.
*   `:root`: Selects the root element of the document (i.e., `<html>`).
*   `:target`: Selects the element that is the target of the URL fragment (e.g., `#section-id`).

```css
button:not(.primary) {
  opacity: 0.8;
}

input[type="checkbox"]:checked + label {
  font-weight: bold;
}
```

---
## 2. Pseudo-elements (`::`)

A pseudo-element is used to style a specified part of an element. They are denoted by a double colon (`::`).

### Common Pseudo-elements:

*   `::first-line`: Styles the first line of a block-level element.
*   `::first-letter`: Styles the first letter of a block-level element.
*   `::before`: Inserts content before the content of an element. Requires the `content` property.
*   `::after`: Inserts content after the content of an element. Requires the `content` property.
*   `::selection`: Styles the portion of an element that is highlighted by the user (e.g., selected text).

```css
p::first-line {
  font-weight: bold;
  color: blue;
}

h1::first-letter {
  font-size: 200%;
  color: #8A2BE2;
}

/* Add an icon before a link */
a.external::before {
  content: "\2192 "; /* Unicode arrow */
  color: gray;
}

/* Add a decorative element after a heading */
h2::after {
  content: " ";
  display: block;
  width: 50px;
  height: 2px;
  background-color: orange;
  margin-top: 5px;
}

/* Style selected text */
::selection {
  background-color: yellow;
  color: black;
}
```

---
## Difference between Pseudo-classes and Pseudo-elements

*   **Pseudo-classes** (`:`): Select elements based on their *state* or *position*.
*   **Pseudo-elements** (`::`): Select and style *parts* of an element.

While older CSS versions used a single colon for both (e.g., `:before`), the double colon (`::`) is the standard for pseudo-elements in CSS3 to distinguish them from pseudo-classes. Browsers generally support both for backward compatibility with older pseudo-elements.

---