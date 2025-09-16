# HTML: Tables

HTML tables are used to display tabular data, meaning data presented in rows and columns. While tables were once used for page layout, their primary purpose now is to structure and present data.

## Basic Table Structure

A basic HTML table is defined with the `<table>` tag. It consists of:

*   `<tr>`: Table Row
*   `<th>`: Table Header (for column headings)
*   `<td>`: Table Data (for actual data cells)

```html
<table>
  <tr>
    <th>Company</th>
    <th>Contact</th>
    <th>Country</th>
  </tr>
  <tr>
    <td>Alfreds Futterkiste</td>
    <td>Maria Anders</td>
    <td>Germany</td>
  </tr>
  <tr>
    <td>Centro comercial Moctezuma</td>
    <td>Francisco Chang</td>
    <td>Mexico</td>
  </tr>
</table>
```

## Table Headers

*   `<th>` elements are used for table headers. By default, they are bold and centered.
*   `<td>` elements are for standard data cells.

## Table Caption

The `<caption>` tag defines a table caption. It should be inserted immediately after the `<table>` tag.

```html
<table>
  <caption>Monthly Savings</caption>
  <tr>
    <th>Month</th>
    <th>Savings</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
  </tr>
</table>
```

## Table Sections (`<thead>`, `<tbody>`, `<tfoot>`)

For better semantic structure and styling, tables can be divided into three sections:

*   `<thead>`: Groups the header content in a table.
*   `<tbody>`: Groups the body content in a table.
*   `<tfoot>`: Groups the footer content in a table.

```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John Doe</td>
      <td>john@example.com</td>
    </tr>
    <tr>
      <td>Jane Smith</td>
      <td>jane@example.com</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="2">Total Users: 2</td>
    </tr>
  </tfoot>
</table>
```

## Spanning Rows and Columns

*   `colspan`: Specifies the number of columns a cell should span horizontally.
*   `rowspan`: Specifies the number of rows a cell should span vertically.

```html
<table>
  <tr>
    <th>Name</th>
    <th colspan="2">Telephone</th>
  </tr>
  <tr>
    <td>Bill Gates</td>
    <td>555 77 854</td>
    <td>555 77 855</td>
  </tr>
</table>

<table>
  <tr>
    <th>Name:</th>
    <td>Bill Gates</td>
  </tr>
  <tr>
    <th rowspan="2">Telephone:</th>
    <td>555 77 854</td>
  </tr>
  <tr>
    <td>555 77 855</td>
  </tr>
</table>
```

## Table Styling (Basic HTML Attributes - Discouraged)

Historically, HTML had attributes like `border`, `cellpadding`, `cellspacing`, `width`, `height` for styling tables. These are now deprecated and should be handled with CSS for better separation of concerns and flexibility.

**Deprecated Example (Avoid in modern HTML):**

```html
<table border="1" cellpadding="10" cellspacing="0" width="100%">
  <!-- ... table content ... -->
</table>
```

**Modern Approach (using CSS):**

```html
<style>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
th, td {
  padding: 8px;
  text-align: left;
}
</style>

<table>
  <!-- ... table content ... -->
</table>
```

## Accessibility in Tables

For accessibility, especially for screen readers, it's important to provide context:

*   Use `<th>` for headers.
*   Use `scope="col"` on `<th>` for column headers and `scope="row"` for row headers.
*   Use `<caption>` for a table title.

```html
<table>
  <caption>Student Grades</caption>
  <thead>
    <tr>
      <th scope="col">Student Name</th>
      <th scope="col">Math</th>
      <th scope="col">Science</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Alice</th>
      <td>90</td>
      <td>85</td>
    </tr>
    <tr>
      <th scope="row">Bob</th>
      <td>75</td>
      <td>92</td>
    </tr>
  </tbody>
</table>
```

Properly structured HTML tables are crucial for presenting data clearly and accessibly on the web.