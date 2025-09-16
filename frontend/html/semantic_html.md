# HTML: Semantic HTML

Semantic HTML refers to the use of HTML markup to reinforce the meaning, or semantics, of the information in web pages rather than merely to define its presentational appearance. For example, `<p>` is a semantic element because it clearly indicates that its content is a paragraph. `<b>` (bold) and `<i>` (italic) are non-semantic because they only define how the text looks, not what it means.

## Why Use Semantic HTML?

*   **Accessibility**: Screen readers and other assistive technologies can better interpret the content and structure of a page, making it more accessible to users with disabilities.
*   **SEO (Search Engine Optimization)**: Search engines can better understand the context and hierarchy of content, potentially leading to improved rankings.
*   **Maintainability**: Code becomes easier to read, understand, and maintain for developers.
*   **Future Compatibility**: Semantic elements are more likely to be understood by future web technologies.

## Key Semantic Elements (HTML5)

HTML5 introduced several new semantic elements to provide better document structure.

### Structural Elements:

*   `<header>`: Represents introductory content, typically containing a group of introductory or navigational aids. It often includes headings, logos, navigation links, and search forms.
*   `<nav>`: Defines a set of navigation links.
*   `<main>`: Represents the dominant content of the `<body>` of a document. It should contain content that is unique to the document and excludes content that is repeated across a set of documents (like sidebars, navigation links, copyright information).
*   `<article>`: Represents a self-contained composition in a document, page, application, or site, which is intended to be independently distributable or reusable (e.g., a forum post, a blog entry, a user-submitted comment, an interactive widget, or any other independent item of content).
*   `<section>`: Represents a standalone section of content, which doesn't have a more specific semantic element to represent it. It typically has a heading.
*   `<aside>`: Represents a portion of a document whose content is only indirectly related to the document's main content. It is often presented as a sidebar or call-out box.
*   `<footer>`: Represents a footer for its nearest sectioning content or sectioning root element. A footer typically contains information about its section, such as who wrote it, links to related documents, copyright data, and the like.

```html
<body>
    <header>
        <h1>Website Title</h1>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">About</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section>
            <h2>Latest Articles</h2>
            <article>
                <h3>Article Title 1</h3>
                <p>Content of article 1...</p>
            </article>
            <article>
                <h3>Article Title 2</h3>
                <p>Content of article 2...</p>
            </article>
        </section>

        <aside>
            <h3>Related Links</h3>
            <ul>
                <li><a href="#">Link 1</a></li>
                <li><a href="#">Link 2</a></li>
            </ul>
        </aside>
    </main>

    <footer>
        <p>&copy; 2023 My Website</p>
    </footer>
</body>
```

### Text-Level Semantic Elements:

*   `<mark>`: Represents text which is marked or highlighted for reference or notation purposes, due to its relevance in the enclosing context.
*   `<time>`: Represents a specific period in time.
*   `<figure>` and `<figcaption>`: Used to group self-contained content (like images, diagrams) and provide a caption for it.
*   `<cite>`: Represents the title of a work (e.g., a book, a song, a film).
*   `<small>`: Represents side comments and small print, like copyright and legal text.
*   `<strong>`: Indicates strong importance, seriousness, or urgency.
*   `<em>`: Indicates emphasis.
*   `<abbr>`: Represents an abbreviation or acronym.
*   `<address>`: Indicates the contact information for its nearest `<article>` or `<body>` ancestor.

```html
<p>I will meet you at <time datetime="2023-10-26T19:00">7 PM</time> tonight.</p>

<figure>
  <img src="image.jpg" alt="Description of image">
  <figcaption>A beautiful sunset.</figcaption>
</figure>

<p>As <cite>The Hitchhiker's Guide to the Galaxy</cite> says, <q>Don't Panic!</q></p>

<p><small>Copyright 2023. All rights reserved.</small></p>

<p><strong>Warning:</strong> Do not proceed without permission.</p>

<p>I <em>really</em> need to learn more about HTML.</p>

<p>The <abbr title="World Health Organization">WHO</abbr> is a specialized agency of the United Nations.</p>

<address>
  Written by John Doe.<br>
  Visit us at:<br>
  Example.com
</address>
```

## Non-Semantic Elements

*   `<div>`: A generic container for flow content. It has no semantic meaning.
*   `<span>`: An inline container for phrasing content. It has no semantic meaning.

These are still useful for styling purposes (e.g., applying CSS to a group of elements) when no other semantic element is appropriate.

```html
<div class="card">
  <h3>Product Title</h3>
  <p>Price: <span class="price">$19.99</span></p>
</div>
```

By choosing the most appropriate HTML element for each piece of content, you create a more meaningful and robust web page.