# CSS: Backgrounds

CSS background properties are used to define the background effects of an element. You can control background color, image, repetition, position, and size.

---
## 1. `background-color`

Sets the background color of an element.

```css
body {
  background-color: #f0f0f0; /* Light gray */
}

div {
  background-color: rgba(255, 99, 71, 0.5); /* Tomato with 50% opacity */
}
```

---
## 2. `background-image`

Sets one or more background images for an element. By default, a background image is repeated both vertically and horizontally.

```css
body {
  background-image: url("paper.gif");
}

div {
  background-image: url("img_tree.png"), url("paper.gif"); /* Multiple backgrounds */
}
```

---
## 3. `background-repeat`

Specifies if/how a background image will be repeated.

*   `repeat` (default): The image is repeated to cover the entire element.
*   `repeat-x`: The image is repeated horizontally only.
*   `repeat-y`: The image is repeated vertically only.
*   `no-repeat`: The image is displayed only once.

```css
body {
  background-image: url("paper.gif");
  background-repeat: no-repeat;
}
```

---
## 4. `background-position`

Sets the starting position of a background image.

Values can be keywords (`top`, `bottom`, `left`, `right`, `center`), percentages, or lengths (`px`, `em`).

```css
body {
  background-image: url("img_tree.png");
  background-repeat: no-repeat;
  background-position: right top; /* Position at top right corner */
}

div {
  background-position: 50% 50%; /* Center horizontally and vertically */
}

.element {
  background-position: 20px 30px; /* 20px from left, 30px from top */
}
```

---
## 5. `background-attachment`

Specifies whether a background image should scroll with the rest of the page or be fixed.

*   `scroll` (default): The background image scrolls with the page.
*   `fixed`: The background image is fixed relative to the viewport.
*   `local`: The background image scrolls with the element's contents.

```css
body {
  background-image: url("img_tree.png");
  background-repeat: no-repeat;
  background-position: right top;
  background-attachment: fixed;
}
```

---
## 6. `background-size`

Specifies the size of the background image(s).

*   `auto` (default): The background image is displayed in its original size.
*   `cover`: Scales the background image to be as large as possible so that the background area is completely covered by the background image. Some parts of the background image may not be in view within the background positioning area.
*   `contain`: Scales the background image to the largest size such that both its width and its height can fit inside the content area.
*   `length` (`px`, `em`, etc.): Sets the width and height (e.g., `200px 100px`).
*   `percentage`: Sets the width and height as a percentage of the parent element.

```css
div {
  background-image: url("img_tree.png");
  background-repeat: no-repeat;
  background-size: cover; /* Cover the entire element */
}

.element-2 {
  background-size: 100% auto; /* 100% width, auto height */
}
```

---
## 7. `background-origin`

Specifies the origin position of a background image (i.e., where the background image is positioned relative to).

*   `padding-box` (default)
*   `border-box`
*   `content-box`

```css
div {
  background-image: url("img_tree.png");
  background-repeat: no-repeat;
  background-origin: content-box;
  padding: 20px;
  border: 10px solid black;
}
```

---
## 8. `background-clip`

Specifies the painting area of the background.

*   `border-box` (default)
*   `padding-box`
*   `content-box`

```css
div {
  background-color: lightblue;
  border: 10px dotted black;
  padding: 20px;
  background-clip: content-box;
}
```

---
## Background Shorthand Property

The `background` shorthand property is used to set all the individual background properties in a single declaration. The order of values is generally:

`background: color image repeat attachment position / size origin clip;`

Not all values are required, and the order for `origin` and `clip` can be tricky.

```css
body {
  background: lightblue url("paper.gif") no-repeat fixed center / cover;
}

/* Equivalent to:
body {
  background-color: lightblue;
  background-image: url("paper.gif");
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: center;
  background-size: cover;
}
*/
```

Using background properties effectively can greatly enhance the visual design of your web pages.

---