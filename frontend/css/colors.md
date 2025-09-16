# CSS: Colors

Colors are a fundamental aspect of web design, used to enhance visual appeal, convey meaning, and improve user experience. CSS provides several ways to define and apply colors to HTML elements.

---
## 1. Color Names

CSS supports 140 standard color names. These are easy to read and understand, but the range is limited.

```css
p {
  color: red;
}

h1 {
  background-color: DodgerBlue;
}

div {
  border: 2px solid MediumSeaGreen;
}
```

Common color names:

*   `red`
*   `blue`
*   `green`
*   `yellow`
*   `black`
*   `white`
*   `gray`
*   `orange`
*   `purple`

---
## 2. RGB (Red, Green, Blue)

RGB color values are specified with `rgb(red, green, blue)`. Each parameter (red, green, blue) defines the intensity of the color as an integer between 0 and 255.

```css
h1 {
  color: rgb(255, 0, 0);   /* Red */
}

p {
  color: rgb(0, 128, 0);  /* Green */
}

div {
  background-color: rgb(255, 99, 71); /* Tomato */
}
```

### RGBA (Red, Green, Blue, Alpha)

RGBA color values extend RGB with an alpha channel, which specifies the opacity of the color. The alpha parameter is a number between 0.0 (fully transparent) and 1.0 (fully opaque).

```css
div.transparent {
  background-color: rgb(0, 0, 0); /* Black */
  opacity: 0.6; /* 60% opacity */
}

div.rgba-transparent {
  background-color: rgba(0, 0, 0, 0.6); /* Black with 60% opacity */
}
```

Using `rgba()` is generally preferred over `opacity` property when you want to make only the background transparent, not the content within the element.

---
## 3. Hexadecimal Colors

Hexadecimal colors are specified with `#RRGGBB`, where RR (red), GG (green), and BB (blue) are hexadecimal values between 00 and FF (equal to 0-255 in decimal).

```css
h1 {
  color: #FF0000;   /* Red */
}

p {
  color: #008000;  /* Green */
}

div {
  background-color: #FF6347; /* Tomato */
}
```

### Shorthand Hexadecimal

If all three pairs of hexadecimal values are the same (e.g., `#FF00FF`), you can use a shorthand notation (`#F0F`).

```css
/* Full hex */
.color1 { color: #FF00CC; }

/* Shorthand hex */
.color2 { color: #F0C; }
```

---
## 4. HSL (Hue, Saturation, Lightness)

HSL color values are specified with `hsl(hue, saturation, lightness)`.

*   **Hue**: A degree on the color wheel (0-360). 0 (or 360) is red, 120 is green, 240 is blue.
*   **Saturation**: A percentage value (0-100%). 0% means a shade of gray, 100% is the full color.
*   **Lightness**: A percentage value (0-100%). 0% is black, 100% is white, 50% is normal.

```css
h1 {
  color: hsl(0, 100%, 50%);   /* Red */
}

p {
  color: hsl(120, 100%, 25%); /* Dark Green */
}

div {
  background-color: hsl(9, 100%, 64%); /* Tomato */
}
```

### HSLA (Hue, Saturation, Lightness, Alpha)

HSLA extends HSL with an alpha channel for opacity, similar to RGBA.

```css
div.hsla-transparent {
  background-color: hsla(0, 0%, 0%, 0.6); /* Black with 60% opacity */
}
```

---
## Color Properties

Colors can be applied to various CSS properties:

*   `color`: Sets the color of the text.
*   `background-color`: Sets the background color of an element.
*   `border-color`: Sets the color of an element's border.
*   `outline-color`: Sets the color of an element's outline.
*   `box-shadow`: Sets the color of a box shadow.
*   `text-shadow`: Sets the color of a text shadow.

```css
.my-element {
  color: #333; /* Text color */
  background-color: #f0f0f0; /* Background color */
  border: 1px solid #ccc; /* Border color */
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2); /* Shadow color */
}
```

Choosing the right color format often depends on the context and desired level of control. Hex and RGB are widely used, while HSL can be more intuitive for designers due to its separation of hue, saturation, and lightness.

---