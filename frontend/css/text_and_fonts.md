# CSS: Text and Fonts

CSS provides extensive properties for styling text and controlling fonts, allowing you to enhance the readability and visual appeal of your web content.

---
## 1. Text Properties

### `color`

Sets the color of the text.

```css
p {
  color: blue;
}
```

### `text-align`

Aligns the text horizontally within its element.

*   `left` (default)
*   `right`
*   `center`
*   `justify` (stretches lines so that each line has equal width)

```css
h1 {
  text-align: center;
}

p {
  text-align: justify;
}
```

### `text-decoration`

Adds or removes decorations from text.

*   `none` (default)
*   `underline`
*   `overline`
*   `line-through`

```css
a {
  text-decoration: none; /* Remove underline from links */
}

h2 {
  text-decoration: underline;
}
```

### `text-transform`

Controls the capitalization of text.

*   `none` (default)
*   `uppercase`
*   `lowercase`
*   `capitalize` (first letter of each word)

```css
h1 {
  text-transform: uppercase;
}

p {
  text-transform: capitalize;
}
```

### `text-indent`

Specifies the indentation of the first line of text in a block of text.

```css
p {
  text-indent: 50px;
}
```

### `letter-spacing`

Increases or decreases the space between characters.

```css
h1 {
  letter-spacing: 3px;
}
```

### `word-spacing`

Increases or decreases the space between words.

```css
p {
  word-spacing: 5px;
}
```

### `line-height`

Sets the height of a line of text.

```css
p {
  line-height: 1.5; /* 1.5 times the font size */
}
```

### `white-space`

Controls how whitespace is handled within an element.

*   `normal` (default)
*   `nowrap` (prevents text from wrapping)
*   `pre` (behaves like `<pre>` tag)

```css
.no-wrap {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis; /* Add ellipsis for overflowed text */
}
```

### `text-shadow`

Adds shadow to text.

```css
h1 {
  text-shadow: 2px 2px 5px red; /* horizontal-shadow vertical-shadow blur-radius color */
}
```

---
## 2. Font Properties

### `font-family`

Specifies the font for an element. It's good practice to provide a fallback font (generic family) in case the preferred font is not available.

```css
p {
  font-family: "Times New Roman", Times, serif;
}

h1 {
  font-family: Arial, sans-serif;
}
```

### `font-size`

Sets the size of the text.

*   `px` (pixels): Absolute size.
*   `em`: Relative to the font-size of the parent element.
*   `rem`: Relative to the font-size of the root (`<html>`) element.
*   `%`: Relative to the parent element's font size.
*   `vw`, `vh`: Relative to viewport width/height.

```css
h1 {
  font-size: 40px;
}

p {
  font-size: 1.2em;
}

.responsive-text {
  font-size: 3vw;
}
```

### `font-weight`

Sets how thick or thin characters in text should be displayed.

*   `normal` (default)
*   `bold`
*   `bolder`
*   `lighter`
*   Numeric values (100, 200, ..., 900)

```css
h1 {
  font-weight: bold;
}

p {
  font-weight: 300; /* Light */
}
```

### `font-style`

Specifies the font style.

*   `normal` (default)
*   `italic`
*   `oblique`

```css
em {
  font-style: normal;
}
```

### `font-variant`

Converts text to small-caps font.

```css
h1 {
  font-variant: small-caps;
}
```

---
## 3. Google Fonts and Web Fonts

To use fonts not installed on the user's computer, you can use web fonts (e.g., Google Fonts).

### Example (Google Fonts):

1.  **Link in HTML `<head>`:**
    ```html
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    ```
2.  **Use in CSS:**
    ```css
    body {
      font-family: 'Roboto', sans-serif;
    }
    ```

---
## 4. Font Shorthand Property

The `font` shorthand property can be used to set all the font properties in one declaration.

`font: font-style font-variant font-weight font-size/line-height font-family;`

```css
p {
  font: italic bold 16px/1.5 Arial, sans-serif;
}

/* Equivalent to:
p {
  font-style: italic;
  font-weight: bold;
  font-size: 16px;
  line-height: 1.5;
  font-family: Arial, sans-serif;
}
*/
```

Mastering text and font styling is crucial for creating visually appealing and readable web designs.

---