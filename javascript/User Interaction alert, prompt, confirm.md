# User Interaction: `alert`, `prompt`, `confirm`

When using the browser as the demo environment, these three built-in functions are commonly used to interact with the user.

Each of them shows a **modal** window — meaning the user *must* interact with it before they can return to the page.

---

## `alert`

The simplest interaction — it displays a message and waits for the user to press “OK.”

```js
alert("Hello");
```

The message appears in a **modal window**, which blocks interaction with the rest of the page until the user clicks “OK.”

---

## `prompt`

`prompt` asks the user for input, displaying a message, an input field, and “OK”/“Cancel” buttons.

### Syntax

```js
result = prompt(title, [default]);
```

* `title` — the text to show in the modal.

* `default` — (optional) initial value for the input field.

* Square brackets in `[default]` indicate that it’s optional.

If the user enters text and clicks OK, the input text is returned.
If the user clicks Cancel or presses Esc, `null` is returned.

### Example

```js
let age = prompt('How old are you?', 100);
alert(`You are ${age} years old!`);
```

### Notes

> In Internet Explorer, if you omit the `default` value, it inserts `"undefined"` into the input field.
> To avoid this, always supply the second parameter:

```js
let test = prompt("Test", '');
```

---

## `confirm`

`confirm` asks a yes/no question, showing a message and “OK”/“Cancel” buttons.

### Syntax

```js
result = confirm(question);
```

If the user clicks OK, it returns `true`.
If the user clicks Cancel, it returns `false`.

### Example

```js
let isBoss = confirm("Are you the boss?");
alert(isBoss); // true if OK is pressed
```

---

#javascript