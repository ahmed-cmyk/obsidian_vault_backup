# HTML: Links

HTML links (hyperlinks) are one of the most fundamental features of the web. They allow users to navigate from one page to another, both within the same website and to external websites. Links are defined with the `<a>` tag.

## The `<a>` Tag

The `<a>` (anchor) tag is used to create a hyperlink. The most important attribute of the `<a>` element is the `href` attribute, which indicates the link's destination.

```html
<a href="url">Link text</a>
```

### Basic Link Example

```html
<p><a href="https://www.google.com">Visit Google</a></p>
<p><a href="/about.html">Learn about us</a></p> <!-- Relative link to a page on the same site -->
```

## The `href` Attribute

The `href` attribute specifies the URL of the page the link goes to.

*   **Absolute URLs**: Link to an external website (e.g., `https://www.example.com/`).
*   **Relative URLs**: Link to a page within the same website (e.g., `/about.html`, `../images/pic.jpg`).

## The `target` Attribute

The `target` attribute specifies where to open the linked document.

*   `_self`: Opens the linked document in the same window/tab as it was clicked (default).
*   `_blank`: Opens the linked document in a new window or tab.
*   `_parent`: Opens the linked document in the parent frame.
*   `_top`: Opens the linked document in the full body of the window.

```html
<p><a href="https://www.w3schools.com/" target="_blank">Visit W3Schools (opens in new tab)</a></p>
```

## Image as a Link

An image can also be used as a link by nesting the `<img>` tag inside the `<a>` tag.

```html
<a href="default.html">
  <img src="smiley.gif" alt="HTML tutorial" style="width:42px;height:42px;">
</a>
```

## Link to an Email Address

Use `mailto:` inside the `href` attribute to create a link that opens the user's email client.

```html
<p><a href="mailto:someone@example.com">Send Email</a></p>
```

## Link to a Phone Number

Use `tel:` inside the `href` attribute to create a link that can dial a phone number on mobile devices.

```html
<p><a href="tel:+15551234567">Call Us</a></p>
```

## Link to a Specific Part of the Same Page (Bookmarks)

To create a link to a specific section within the same page, you need two parts:

1.  An `id` attribute on the element you want to link to.
2.  An `<a>` tag with `href` pointing to that `id` (prefixed with `#`).

```html
<!-- Target element -->
<h2 id="section1">Section One</h2>
<p>Content of section one...</p>

<!-- Link to the target -->
<p><a href="#section1">Go to Section One</a></p>
```

## Link to a Specific Part of Another Page

You can also link to a specific section on another page.

```html
<p><a href="another_page.html#section2">Go to Section Two on Another Page</a></p>
```

## Download Links

Use the `download` attribute to suggest that the browser download the linked resource rather than navigating to it.

```html
<a href="/files/document.pdf" download>Download PDF</a>
<a href="/images/picture.jpg" download="my_picture.jpg">Download Picture</a>
```

## Base URL for Relative Links

The `<base>` element specifies the base URL/target for all relative URLs in a document. It must be inside the `<head>` section.

```html
<head>
  <base href="https://www.w3schools.com/images/" target="_blank">
</head>
<body>
  <img src="html5.gif" width="24" height="9" alt="HTML5">
  <a href="tags/tag_base.asp">HTML base Tag</a>
</body>
```

In this example, the `<img>` source will resolve to `https://www.w3schools.com/images/html5.gif`, and the `<a>` link will open `https://www.w3schools.com/tags/tag_base.asp` in a new tab.