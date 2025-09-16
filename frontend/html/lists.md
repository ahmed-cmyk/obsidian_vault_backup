# HTML: Lists

HTML provides several ways to group a set of related items into lists. There are three main types of lists:

1.  **Unordered Lists** (`<ul>`)
2.  **Ordered Lists** (`<ol>`)
3.  **Description Lists** (`<dl>`)

## 1. Unordered Lists (`<ul>`)

An unordered list is used to group a set of related items in no particular order. Each item in the list is marked with a bullet point (by default).

*   The `<ul>` tag defines the unordered list.
*   The `<li>` (list item) tag defines each item in the list.

```html
<h3>My Favorite Fruits:</h3>
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Cherry</li>
</ul>
```

### Styling Unordered Lists

You can change the bullet style using CSS (e.g., `list-style-type: circle;`, `square;`, `none;`).

## 2. Ordered Lists (`<ol>`)

An ordered list is used to group a set of related items in a specific order. Each item in the list is marked with a number or letter (by default).

*   The `<ol>` tag defines the ordered list.
*   The `<li>` (list item) tag defines each item in the list.

```html
<h3>Steps to Make Coffee:</h3>
<ol>
  <li>Boil water</li>
  <li>Add coffee grounds to filter</li>
  <li>Pour hot water over grounds</li>
  <li>Enjoy!</li>
</ol>
```

### Attributes for Ordered Lists

*   `type`: Specifies the type of marker to use.
    *   `1` (default): Numbers (1, 2, 3...)
    *   `A`: Uppercase letters (A, B, C...)
    *   `a`: Lowercase letters (a, b, c...)
    *   `I`: Uppercase Roman numerals (I, II, III...)
    *   `i`: Lowercase Roman numerals (i, ii, iii...)

*   `start`: Specifies the start value of the first list item.

*   `reversed`: Specifies that the list order should be descending.

```html
<ol type="A">
  <li>Item A</li>
  <li>Item B</li>
</ol>

<ol start="5">
  <li>Item 5</li>
  <li>Item 6</li>
</ol>

<ol reversed>
  <li>Item 3</li>
  <li>Item 2</li>
  <li>Item 1</li>
</ol>
```

## 3. Description Lists (`<dl>`)

A description list (or definition list) is a list of terms, with a description of each term.

*   The `<dl>` tag defines the description list.
*   The `<dt>` (description term) tag defines the term.
*   The `<dd>` (description details) tag describes each term.

```html
<h3>Glossary:</h3>
<dl>
  <dt>HTML</dt>
  <dd>HyperText Markup Language, the standard markup language for creating web pages.</dd>

  <dt>CSS</dt>
  <dd>Cascading Style Sheets, used for describing the presentation of a document written in HTML.</dd>

  <dt>JavaScript</dt>
  <dd>A programming language that enables interactive web pages.</dd>
</dl>
```

## Nested Lists

You can nest lists within other lists to create hierarchical structures.

```html
<h3>Menu:</h3>
<ul>
  <li>Coffee</li>
  <li>Tea
    <ul>
      <li>Black Tea</li>
      <li>Green Tea
        <ol>
          <li>Sencha</li>
          <li>Matcha</li>
        </ol>
      </li>
    </ul>
  </li>
  <li>Milk</li>
</ul>
```

Using the appropriate list type helps convey the semantic meaning of your content, which is beneficial for accessibility and readability.