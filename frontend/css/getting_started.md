# CSS: Getting Started

CSS (Cascading Style Sheets) is a stylesheet language used to describe the presentation of a document written in HTML or XML (including XML dialects such as SVG, MathML or XHTML). CSS describes how elements should be rendered on screen, on paper, in speech, or on other media.

---
## What is CSS?

*   **Cascading**: Refers to the way CSS rules are applied, with rules from different sources (browser, user, author) and specificities overriding each other.
*   **Style Sheets**: Documents that define the style of a web page.

CSS is used to style the layout of web pages, including colors, fonts, spacing, and responsive behavior.

---
## How to Add CSS to HTML

There are three ways to insert a style sheet:

### 1. External Style Sheet (Recommended)

An external style sheet is ideal when the style is applied to many pages. With an external style sheet, you can change the look of an entire website by changing just one file.

*   Create a `.css` file (e.g., `styles.css`).
*   Link it in the `<head>` section of your HTML document using the `<link>` tag.

**`styles.css`:**
```css
body {
  background-color: lightblue;
}

h1 {
  color: navy;
  margin-left: 20px;
}
```

**`index.html`:**
```html
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" href="styles.css">
</head>
<body>

<h1>This is a heading</h1>
<p>This is a paragraph.</p>

</body>
</html>
```

### 2. Internal Style Sheet

An internal style sheet may be used if one single HTML page has a unique style. Internal styles are defined within the `<style>` element, inside the `<head>` section of an HTML page.

**`index.html`:**
```html
<!DOCTYPE html>
<html>
<head>
<style>
body {
  background-color: lightblue;
}

h1 {
  color: navy;
  margin-left: 20px;
}
</style>
</head>
<body>

<h1>This is a heading</h1>
<p>This is a paragraph.</p>

</body>
</html>
```

### 3. Inline Styles

An inline style may be used to apply a unique style for a single HTML element. To use inline styles, add the `style` attribute to the relevant HTML tag. The `style` attribute can contain any CSS property.

**`index.html`:**
```html
<!DOCTYPE html>
<html>
<body>

<h1 style="color:blue;text-align:center;">This is a Heading</h1>
<p style="color:red;">This is a paragraph.</p>

</body>
</html>
```

---
## CSS Syntax

A CSS rule-set consists of a selector and a declaration block:

```css
selector {
  property: value;
  property: value;
}
```

*   **Selector**: Points to the HTML element you want to style (e.g., `h1`, `p`, `.class`, `#id`).
*   **Declaration Block**: Contains one or more declarations separated by semicolons.
*   **Declaration**: Includes a CSS property name and a value, separated by a colon.

Example:

*   `h1` is the selector.
*   `color` is the property, and `blue` is the value.
*   `font-size` is the property, and `12px` is the value.

```css
h1 {
  color: blue;
  font-size: 12px;
}
```

---
## Comments in CSS

Comments in CSS are used to explain the code and are ignored by the browser. They are enclosed within `/*` and `*/`.

```css
/* This is a single-line CSS comment */
p {
  color: red; /* This is an inline comment */
}

/*
This is a multi-line
CSS comment
*/
```

Understanding these basics is the first step to effectively styling your web pages with CSS.

---