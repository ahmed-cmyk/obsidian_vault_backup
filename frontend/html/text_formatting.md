# HTML: Text Formatting

HTML provides a variety of elements to format text, giving it semantic meaning and controlling its appearance (though appearance is primarily handled by CSS). These elements help structure content and convey its purpose.

## Headings

HTML headings are defined with the `<h1>` to `<h6>` tags. `<h1>` defines the most important heading, and `<h6>` defines the least important heading.

```html
<h1>This is Heading 1</h1>
<h2>This is Heading 2</h2>
<h3>This is Heading 3</h3>
<h4>This is Heading 4</h4>
<h5>This is Heading 5</h5>
<h6>This is Heading 6</h6>
```

*   **Semantic Use**: Headings should be used to structure your content hierarchically, not just for visual size. There should typically be only one `<h1>` per page, representing the main title.

## Paragraphs

HTML paragraphs are defined with the `<p>` tag.

```html
<p>This is a paragraph of text.</p>
<p>This is another paragraph.</p>
```

## Line Breaks and Horizontal Rules

*   `<br>`: Inserts a single line break. It's an empty tag.
*   `<hr>`: Defines a thematic break in an HTML page, most often displayed as a horizontal rule. It's also an empty tag.

```html
<p>This is some text.<br>This is on a new line.</p>
<hr>
<p>This is text after a horizontal rule.</p>
```

## Text Styling Elements

These elements apply specific semantic meaning or visual styling to text.

*   `<b>`: **Bold text** (semantic meaning: stylistically offset, but not important).
*   `<strong>`: **Important text** (semantic meaning: strong importance).
*   `<i>`: *Italic text* (semantic meaning: alternate voice, technical term, etc.).
*   `<em>`: *Emphasized text* (semantic meaning: emphasized).
*   `<mark>`: <mark>Marked/highlighted text</mark>.
*   `<small>`: <small>Smaller text</small>.
*   `<del>`: <del>Deleted text</del>.
*   `<ins>`: <ins>Inserted text</ins>.
*   `<sub>`: <sub>Subscript text</sub>.
*   `<sup>`: <sup>Superscript text</sup>.

```html
<p>This is <b>bold</b> text.</p>
<p>This is <strong>important</strong> text.</p>
<p>This is <i>italic</i> text.</p>
<p>This is <em>emphasized</em> text.</p>
<p>This is <mark>marked</mark> text.</p>
<p>This is <small>small</small> text.</p>
<p>This is <del>deleted</del> text.</p>
<p>This is <ins>inserted</ins> text.</p>
<p>This is <sub>subscript</sub> text.</p>
<p>This is <sup>superscript</sup> text.</p>
```

## Quotations and Citations

*   `<blockquote cite="url">`: For long quotations that are block-level. The `cite` attribute can point to the source.
*   `<q>`: For short inline quotations.
*   `<abbr title="full form">`: For abbreviations.
*   `<address>`: For contact information for the nearest `article` or `body` ancestor.
*   `<cite>`: For the title of a creative work (e.g., a book, song, film).
*   `<bdo dir="rtl">`: Overrides the current text direction.

```html
<p>Here is a quote from a famous person:</p>
<blockquote cite="http://www.example.com/quote-source">
  <p>The only way to do great work is to love what you do.</p>
  <footer>— <cite>Steve Jobs</cite></footer>
</blockquote>

<p>As <q>to be or not to be</q> is the question.</p>

<p>The <abbr title="World Health Organization">WHO</abbr> was founded in 1948.</p>

<address>
  Written by John Doe.<br>
  Visit us at:<br>
  Example.com<br>
  Box 564, Disneyland<br>
  USA
</address>

<p><bdo dir="rtl">This text will be written from right to left.</bdo></p>
```

## Code and Preformatted Text

*   `<code>`: Used to display a short fragment of computer code.
*   `<pre>`: Defines preformatted text. Text inside a `<pre>` element is displayed in a fixed-width font, and it preserves both spaces and line breaks.
*   `<var>`: Represents a variable in a mathematical expression or programming context.
*   `<kbd>`: Represents user input (typically keyboard input).
*   `<samp>`: Represents sample output from a computer program.

```html
<p>The `print()` function is often used.</p>

<pre>
function greet() {
    console.log("Hello, World!");
}
</pre>

<p>If <var>x</var> is greater than <var>y</var>, then...</p>

<p>Press <kbd>Ctrl</kbd> + <kbd>S</kbd> to save.</p>

<p>The program returned: <samp>Error: File not found.</samp></p>
```

Using these HTML text formatting elements correctly enhances the semantic meaning of your content, which is beneficial for accessibility, SEO, and maintainability.