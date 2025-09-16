# CSS: Positioning

The CSS `position` property controls how an element is positioned on a web page. It works in conjunction with the `top`, `bottom`, `left`, and `right` properties to determine the final location of the positioned element.

Understanding positioning is crucial for creating complex layouts, overlays, and precise element placement.

---
## 1. `position: static` (Default)

*   HTML elements are positioned `static` by default.
*   Static-positioned elements are not affected by the `top`, `bottom`, `left`, and `right` properties.
*   They are positioned according to the normal flow of the page.

```css
div.static {
  position: static;
  border: 3px solid #73AD21;
}
```

---
## 2. `position: relative`

*   A relatively positioned element is positioned relative to its normal position.
*   Setting the `top`, `right`, `bottom`, and `left` properties of a relatively-positioned element will cause it to be adjusted away from its normal position. Other content will not be adjusted to fit into any gap left by the element.
*   Crucially, `position: relative` creates a new positioning context for its children. This means absolutely positioned children will be positioned relative to this parent.

```css
div.relative {
  position: relative;
  left: 30px;
  top: 20px;
  border: 3px solid #73AD21;
}
```

---
## 3. `position: absolute`

*   An absolutely positioned element is positioned relative to its *nearest positioned ancestor* (an ancestor with `position` other than `static`).
*   If an absolutely positioned element has no positioned ancestors, it uses the document body, and still moves along with page scrolling.
*   Absolute-positioned elements are removed from the normal document flow. This means they do not take up space and can overlap other elements.

```css
div.relative-parent {
  position: relative;
  width: 400px;
  height: 200px;
  border: 3px solid #73AD21;
}

div.absolute-child {
  position: absolute;
  top: 50px;
  right: 0;
  width: 100px;
  height: 100px;
  background-color: lightblue;
}
```

---
## 4. `position: fixed`

*   A fixed-positioned element is positioned relative to the viewport, which means it always stays in the same place even if the page is scrolled.
*   The `top`, `right`, `bottom`, and `left` properties are used to position the element.
*   Fixed elements do not take up space in the normal document flow.

```css
div.fixed {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 200px;
  border: 3px solid #73AD21;
  background-color: yellow;
}
```

---
## 5. `position: sticky`

*   A sticky-positioned element is positioned based on the user's scroll position.
*   It toggles between `relative` and `fixed`, depending on the scroll position. It is positioned `relative` until a given offset position is met in the viewport, then it "sticks" in place (like `fixed`).

```css
div.sticky {
  position: -webkit-sticky; /* Safari */
  position: sticky;
  top: 0; /* Sticks to the top of the viewport when scrolled */
  background-color: green;
  padding: 10px;
  border: 2px solid #4CAF50;
}
```

---
## `z-index` Property

When elements are positioned, they can overlap. The `z-index` property specifies the stack order of an element (which element should be placed in front of, or behind, others).

*   An element with a higher `z-index` value is always in front of an element with a lower `z-index` value.
*   `z-index` only works on positioned elements (`position: absolute`, `relative`, `fixed`, or `sticky`).

```css
.box1 {
  position: absolute;
  left: 0;
  top: 0;
  z-index: 2; /* Will be on top */
  background-color: red;
}

.box2 {
  position: absolute;
  left: 20px;
  top: 20px;
  z-index: 1; /* Will be behind */
  background-color: blue;
}
```

Mastering the `position` property and its related properties (`top`, `right`, `bottom`, `left`, `z-index`) is essential for fine-grained control over element placement and creating dynamic visual effects on your web pages.

---