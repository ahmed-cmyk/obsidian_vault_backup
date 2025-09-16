# HTML: Document Structure

The basic structure of an HTML document is crucial for defining the content and metadata of a web page. It consists of several key elements that browsers use to render the page correctly.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>
    <link rel="stylesheet" href="styles.css">
    <script src="script.js"></script>
</head>
<body>

    <header>
        <h1>Website Header</h1>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">About</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section>
            <h2>Section Title</h2>
            <p>This is a section of content.</p>
            <article>
                <h3>Article Title</h3>
                <p>This is an article within the section.</p>
            </article>
        </section>

        <aside>
            <h4>Sidebar Content</h4>
            <p>Related information or advertisements.</p>
        </aside>
    </main>

    <footer>
        <p>&copy; 2023 My Website</p>
    </footer>

</body>
</html>
```

## Key Structural Elements

### `<!DOCTYPE html>`

*   **Purpose**: This declaration defines the document type and version of HTML. For HTML5, it's simply `<!DOCTYPE html>`. It's not an HTML tag but an instruction to the web browser about what version of HTML the page is written in.

### `<html>`

*   **Purpose**: The root element of an HTML page. All other HTML elements are descendants of this element.
*   **Attributes**: Often includes the `lang` attribute (e.g., `lang="en"`) to declare the language of the document, which is important for accessibility and search engines.

### `<head>`

*   **Purpose**: Contains meta-information about the HTML document that is not displayed on the web page itself. This includes metadata, links to stylesheets, and scripts.
*   **Common elements within `<head>`:**
    *   `<meta charset="UTF-8">`: Specifies the character encoding for the document, ensuring proper display of various characters.
    *   `<meta name="viewport" content="width=device-width, initial-scale=1.0">`: Configures the viewport for responsive design, making the page look good on all devices.
    *   `<title>`: Sets the title of the web page, which appears in the browser tab or window title bar.
    *   `<link>`: Used to link external resources, most commonly stylesheets (`<link rel="stylesheet" href="styles.css">`).
    *   `<script>`: Used to embed or link JavaScript code.
    *   `<style>`: Used to embed CSS directly within the HTML document.

### `<body>`

*   **Purpose**: Contains all the visible content of the web page. Everything you see in the browser window (text, images, videos, links, etc.) is placed within the `<body>` tags.

## Semantic HTML5 Structure Elements

HTML5 introduced several new semantic elements to better describe the structure of a web page. Using these elements improves accessibility and SEO.

### `<header>`

*   **Purpose**: Represents introductory content, typically containing a group of introductory or navigational aids. It often includes headings, logos, navigation links, and search forms.

### `<nav>`

*   **Purpose**: Defines a set of navigation links.

### `<main>`

*   **Purpose**: Represents the dominant content of the `<body>` of a document. It should contain content that is unique to the document and excludes content that is repeated across a set of documents (like sidebars, navigation links, copyright information).

### `<section>`

*   **Purpose**: Represents a standalone section of content, which doesn't have a more specific semantic element to represent it. It typically has a heading.

### `<article>`

*   **Purpose**: Represents a self-contained composition in a document, page, application, or site, which is intended to be independently distributable or reusable (e.g., a forum post, a blog entry, a user-submitted comment, an interactive widget, or any other independent item of content).

### `<aside>`

*   **Purpose**: Represents a portion of a document whose content is only indirectly related to the document's main content. It is often presented as a sidebar or call-out box.

### `<footer>`

*   **Purpose**: Represents a footer for its nearest sectioning content or sectioning root element. A footer typically contains information about its section, such as who wrote it, links to related documents, copyright data, and the like.

Understanding and correctly using these structural elements is key to building well-formed, accessible, and maintainable HTML documents.