# HTML: Getting Started

HTML (HyperText Markup Language) is the standard markup language for creating web pages. It describes the structure of a web page semantically and originally included cues for the appearance of the document.

## What is HTML?

*   **HyperText**: Refers to the way web pages are linked together. Text that contains links to other texts.
*   **Markup Language**: A computer language that uses tags to define elements within a document.

HTML is not a programming language; it cannot perform dynamic functionality. It is a markup language that defines the structure of your content.

## Basic HTML Document Structure

Every HTML document should have a basic structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My First HTML Page</title>
</head>
<body>

    <h1>Hello, World!</h1>
    <p>This is my first paragraph.</p>

</body>
</html>
```

### Explanation of Tags:

*   `<!DOCTYPE html>`: Declaration defines this document to be an HTML5 document.
*   `<html lang="en">`: The root element of an HTML page. The `lang="en"` attribute specifies the language of the document, which is good for accessibility and SEO.
*   `<head>`: Contains meta-information about the HTML document, such as character set, viewport settings, and the page title. This content is not displayed on the web page itself.
    *   `<meta charset="UTF-8">`: Specifies the character encoding for the document, ensuring proper display of various characters.
    *   `<meta name="viewport" content="width=device-width, initial-scale=1.0">`: Configures the viewport for responsive design, making the page look good on all devices.
    *   `<title>`: Sets the title of the web page, which appears in the browser tab or window title bar.
*   `<body>`: Contains the visible page content, such as headings, paragraphs, images, links, etc.
    *   `<h1>`: Defines a main heading.
    *   `<p>`: Defines a paragraph.

## Creating Your First HTML File

1.  **Open a text editor**: Use any plain text editor (e.g., VS Code, Sublime Text, Notepad, TextEdit).
2.  **Save the file**: Save the content above as `index.html` (or any name ending with `.html` or `.htm`).
3.  **Open in browser**: Double-click the saved file, and it will open in your default web browser.

## HTML Elements

An HTML element usually consists of a start tag, an end tag, and the content in between:

`<tagname>Content goes here...</tagname>`

Some HTML elements are empty elements (self-closing) and do not have an end tag:

`<br>` (line break)
`<img src="image.jpg" alt="Description">` (image)

## HTML Attributes

Attributes provide additional information about HTML elements. They are always specified in the start tag and usually come in name/value pairs like: `name="value"`.

Example: `<a href="https://www.example.com">Visit Example</a>`

*   `href` is the attribute name.
*   `"https://www.example.com"` is the attribute value.

## Comments in HTML

Comments are used to add notes to your HTML code. They are ignored by the browser and are not displayed on the web page. They are useful for explaining your code.

```html
<!-- This is an HTML comment -->
<p>This is a paragraph.</p>
<!-- Another comment, spanning multiple lines.
     This helps in understanding complex sections. -->
```

This basic understanding forms the foundation for building more complex web pages with HTML.