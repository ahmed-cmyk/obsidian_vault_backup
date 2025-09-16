# HTML: Images

Images are essential for making web pages visually appealing and informative. HTML provides the `<img>` tag to embed images into a document.

## The `<img>` Tag

The `<img>` tag is used to embed an image in an HTML page. It is a self-closing (empty) tag, meaning it has no closing tag.

```html
<img src="url" alt="alternatetext">
```

### Essential Attributes:

*   `src` (Source): Specifies the path to the image file. This is a required attribute.
*   `alt` (Alternative Text): Provides an alternate text for the image if the image cannot be displayed (e.g., due to slow connection, error in `src`, or for screen readers). This is a crucial attribute for accessibility and SEO.

### Example:

```html
<img src="img_girl.jpg" alt="Girl in a jacket">
```

## Image Paths

Just like links, image paths can be absolute or relative.

*   **Absolute URL**: Links to an image on another website.
    ```html
    <img src="https://www.w3schools.com/images/w3html.gif" alt="W3Schools.com">
    ```
*   **Relative URL**: Links to an image within your own website.
    *   Image in the same folder:
        ```html
        <img src="my_picture.jpg" alt="My Picture">
        ```
    *   Image in a subfolder (e.g., `images/`):
        ```html
        <img src="images/my_picture.jpg" alt="My Picture">
        ```
    *   Image in a parent folder:
        ```html
        <img src="../my_picture.jpg" alt="My Picture">
        ```

## Image Dimensions (`width` and `height`)

You can specify the width and height of an image using the `width` and `height` attributes. It's good practice to include these to prevent layout shifts while the image loads.

```html
<img src="img_girl.jpg" alt="Girl in a jacket" width="500" height="600">
```

*   **Best Practice**: While you can set dimensions in HTML, it's often better to control image sizing using CSS for more flexibility and responsiveness.

## Image as a Link

To make an image clickable, wrap the `<img>` tag inside an `<a>` tag.

```html
<a href="default.html">
  <img src="smiley.gif" alt="HTML tutorial" style="width:42px;height:42px;">
</a>
```

## Image Maps

An image map allows you to create clickable areas on an image. Each clickable area is defined by `<area>` tags within a `<map>` element.

*   The `usemap` attribute of the `<img>` tag refers to the `id` of the `<map>` element.
*   The `<area>` tag defines the clickable regions.
    *   `shape`: `rect`, `circle`, `poly` (polygon), or `default`.
    *   `coords`: Coordinates for the shape.
    *   `href`: The link for that area.

```html
<img src="workplace.jpg" alt="Workplace" usemap="#workmap" width="400" height="379">

<map name="workmap">
  <area shape="rect" coords="34,44,270,350" alt="Computer" href="computer.htm">
  <area shape="rect" coords="290,172,333,250" alt="Phone" href="phone.htm">
  <area shape="circle" coords="337,300,44" alt="Cup of coffee" href="coffee.htm">
</map>
```

## The `<picture>` Element (Responsive Images)

The `<picture>` element allows you to display different images for different screen sizes or device resolutions. It contains multiple `<source>` elements and one `<img>` element.

*   The browser will choose the first `<source>` element with matching `media` or `type` attributes.
*   The `<img>` element is a fallback for browsers that don't support `<picture>` or if no `<source>` matches.

```html
<picture>
  <source media="(min-width: 650px)" srcset="img_pink_flowers.jpg">
  <source media="(min-width: 465px)" srcset="img_white_flower.jpg">
  <img src="img_small_flower.jpg" alt="Flowers" style="width:auto;">
</picture>
```

## Figure and Figcaption

The `<figure>` element is used to group self-contained content, like images, diagrams, code listings, etc., that are referenced from the main flow of a document. The `<figcaption>` element provides a caption for the `<figure>` element.

```html
<figure>
  <img src="pic_trulli.jpg" alt="Trulli" style="width:100%">
  <figcaption>Fig.1 - Trulli, Puglia, Italy.</figcaption>
</figure>
```

Using images effectively enhances the user experience, but it's important to optimize them for web performance and ensure accessibility with proper `alt` text.