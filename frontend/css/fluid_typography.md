
If you don't specify a line length, browsers will wrap lines of text at the edge of the screen. So text on the web is responsive by default—it flows to fit the user's viewport.

> **NOTE:** Just because text fits on a screen doesn't mean it's comfortable to read. Good typography is all about presenting your text in an appropriate way.

---
## Text Size

Use smaller text sizes for smaller screens and larger text sizes for larger screens. You can use media queries to change the `font-size` property as the screen size gets wider.

```css
@media (min-width: 30em) {
  html {
    font-size: 125%;
  }
}

@media (min-width: 40em) {
  html {
    font-size: 150%;
  }
}

@media (min-width: 50em) {
  html {
    font-size: 175%;
  }
}

@media (min-width: 60em) {
  html {
    font-size: 200%;
  }
}
```

> **NOTE:** The problem with using media queries is that it tends to be quite jumpy. A better and more responsive approach is to use the user's device width to influence the text size.  

The best way to handle responsive text based on the viewport width is to combine both `vw` and `em`, `rem`, and `ch` units. You can use the `calc()` function for this.

```css
html {
  font-size: calc(0.75rem + 1.5vw);
}
```

---
## Control Scaling Start and End

The `clamp()` function is like the `calc()` function but it takes *three values*. 

- The middle value is the same as what you pass to `calc()`. 
- The opening value specifies the minimum size, in this case `1rem` so as to not go below the user's preferred font size. 
- The closing value specifies the maximum size.

```css
html {
  font-size: clamp(1rem, 0.75rem + 1.5vw, 2rem);
}
```

---
## Line Length


The web is not print, but we can borrow lessons from print design.

In _The Elements of Typographic Style_, Robert Bringhurst recommends:

- **45–75 characters** per line for single-column text.    
- **66 characters** is considered ideal.
- **40–50 characters** per line for multi-column layouts.

There is no line-length property in CSS, but you can control text width with max-inline-size.

**Don’t** use fixed units like px. Use relative units like ch (characters) so text reflows with the user’s font size.

```css
/* Bad */
article {
  max-inline-size: 700px;
}

/* Good */
article {
  max-inline-size: 66ch;
}
```

> **NOTE:** Using `ch` ensures that lines wrap around the target character count, regardless of font size.

---
## Line Height

Readable text also depends on line height.

- Shorter lines can handle larger line-height.
- Longer lines should use tighter line-height to avoid eye strain.

```css
article {
  max-inline-size: 66ch;
  line-height: 1.65;
}

blockquote {
  max-inline-size: 45ch;
  line-height: 2;
}
```

> **NOTE:** Always use **unitless values** for line-height. This keeps it proportional to the current font-size.

```css
/* Bad */
line-height: 24px;

/* Good */
line-height: 1.5;
```

---
## Combinations and Scale

- Establish a **typography scale** in your design system.
- Use it to maintain **hierarchy, clarity, and flow** across the page.

---
## Web Fonts

A typeface is the _voice_ of your words.

- Use @font-face with **WOFF2** for performance.
- Define a fallback stack (Roboto, sans-serif).
- Remember: each font file increases load time.

```css
@font-face {
  font-family: Roboto;
  src: url('/fonts/roboto-regular.woff2') format('woff2');
}

body {
  font-family: Roboto, sans-serif;
}
```

> **NOTE:** Good typography balances **aesthetic quality** with **performance**.

---
## Font Loading

Speed matters. Fonts load asynchronously, so you must decide how text appears during loading.

- Use `<link rel="preload" as="font">` to prioritize font files.
- Always add crossorigin.

```html
<link href="/fonts/roboto-regular.woff2" type="font/woff2" 
  rel="preload" as="font" crossorigin>
```

- Use font-display to control swap behavior:

```css
/* Text swaps as soon as font loads */
body {
  font-family: Roboto, sans-serif;
  font-display: swap;
}

/* System font stays once rendered */
body {
  font-family: Roboto, sans-serif;
  font-display: fallback;
}
```

- Use size-adjust to reduce layout shift when fonts swap.

---
## Variable Fonts

- Replace multiple files (regular, bold, italic) with a **single variable font**.
- Provides **weight and style flexibility** in one file.
- Usually more performant if you use multiple weights.

```css
body {
  font-family: 'InterVariable', system-ui, sans-serif;
}
```

> **NOTE:** Many system fonts (system-ui) are already variable fonts, meaning you get flexibility without downloads.

---
