# HTML: Forms

HTML forms are used to collect user input. The input is most often sent to a server for processing. Forms are a fundamental part of interactive web pages.

## The `<form>` Element

The `<form>` element is a container for different types of input elements, such as text fields, checkboxes, radio buttons, submit buttons, etc.

```html
<form action="/submit_form.php" method="post">
  <!-- Input elements go here -->
</form>
```

### Attributes of `<form>`:

*   `action`: Specifies the URL where the form data will be sent when submitted. If omitted, the data is sent to the current page.
*   `method`: Specifies the HTTP method to use when sending form data.
    *   `get`: Appends form data to the URL in name/value pairs. Data is visible in the URL. Suitable for non-sensitive data and when users might want to bookmark the result.
    *   `post`: Sends form data as an HTTP POST transaction. Data is not visible in the URL. Suitable for sensitive data (passwords) or large amounts of data.
*   `enctype`: Specifies how the form data should be encoded when submitted (only for `method="post"`).
    *   `application/x-www-form-urlencoded` (default): All characters are encoded before sent.
    *   `multipart/form-data`: Used when uploading files.
*   `target`: Specifies where to display the response after submitting the form (e.g., `_blank` for a new tab).

## Input Elements

The `<input>` element is the most important form element. It can be displayed in several ways, depending on the `type` attribute.

### Common Input Types:

*   `text`: Single-line text input.
    ```html
    <label for="fname">First name:</label><br>
    <input type="text" id="fname" name="fname"><br>
    ```
*   `password`: Password field (characters are masked).
    ```html
    <label for="pwd">Password:</label><br>
    <input type="password" id="pwd" name="pwd"><br>
    ```
*   `submit`: A button for submitting the form.
    ```html
    <input type="submit" value="Submit">
    ```
*   `radio`: Radio buttons (allow only one selection from a group).
    ```html
    <input type="radio" id="html" name="fav_language" value="HTML">
    <label for="html">HTML</label><br>
    <input type="radio" id="css" name="fav_language" value="CSS">
    <label for="css">CSS</label><br>
    ```
*   `checkbox`: Checkboxes (allow multiple selections).
    ```html
    <input type="checkbox" id="vehicle1" name="vehicle1" value="Bike">
    <label for="vehicle1"> I have a bike</label><br>
    <input type="checkbox" id="vehicle2" name="vehicle2" value="Car">
    <label for="vehicle2"> I have a car</label><br>
    ```
*   `email`: Email address input (basic validation).
*   `number`: Numeric input (with min/max attributes).
*   `date`: Date picker.
*   `file`: File upload control.
*   `hidden`: Hidden input field (not visible to user, but submitted with form).

### Attributes for `<input>`:

*   `name`: **Crucial!** The name of the input field, used to identify the data on the server-side.
*   `id`: Unique identifier for the element, used with `<label>` for accessibility and by JavaScript.
*   `value`: The initial value of the input field.
*   `placeholder`: A short hint that describes the expected value of an input field.
*   `required`: Specifies that an input field must be filled out before submitting the form.
*   `readonly`: Specifies that an input field is read-only (cannot be changed).
*   `disabled`: Specifies that an input field is disabled (cannot be used and not submitted).
*   `min`, `max`: For `number` and `date` types, specifies minimum and maximum allowed values.
*   `maxlength`: Specifies the maximum number of characters allowed in a text input.

## The `<label>` Element

It's good practice to use the `<label>` tag with input fields. The `for` attribute of the `<label>` should match the `id` of the input element. This improves accessibility, as clicking the label will focus the associated input.

```html
<label for="username">Username:</label>
<input type="text" id="username" name="username">
```

## Other Form Elements

### `<textarea>`

Defines a multi-line text input control.

```html
<label for="comment">Comments:</label><br>
<textarea id="comment" name="comment" rows="4" cols="50"></textarea>
```

### `<select>`

Defines a drop-down list.

*   `<option>`: Defines an option in the list.

```html
<label for="country">Choose a country:</label>
<select id="country" name="country">
  <option value="usa">USA</option>
  <option value="canada">Canada</option>
  <option value="uk" selected>United Kingdom</option>
</select>
```

### `<button>`

Defines a clickable button. More flexible than `<input type="submit">` as it can contain HTML content.

```html
<button type="submit">Send Form</button>
<button type="reset">Reset Form</button>
<button type="button">Just a Button</button>
```

### `<fieldset>` and `<legend>`

Used to group related elements in a form.

*   `<fieldset>`: Draws a box around the related elements.
*   `<legend>`: Defines a caption for the `<fieldset>` element.

```html
<fieldset>
  <legend>Personal Information:</legend>
  <label for="fname">First name:</label><br>
  <input type="text" id="fname" name="fname"><br>
  <label for="lname">Last name:</label><br>
  <input type="text" id="lname" name="lname"><br>
</fieldset>
```

## HTML5 Form Validation

HTML5 introduced built-in form validation, which can be triggered by attributes like `required`, `type="email"`, `min`, `max`, `pattern`, etc. This provides basic client-side validation before the form is submitted to the server.

```html
<input type="email" name="email" required>
<input type="number" name="quantity" min="1" max="10">
<input type="text" name="zip" pattern="[0-9]{5}" title="Five digit zip code">
```

While HTML5 validation is useful for user experience, **server-side validation is always necessary** for security and data integrity, as client-side validation can be bypassed.