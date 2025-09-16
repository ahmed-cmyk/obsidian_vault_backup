# HTML: HTML5 Features

HTML5 is the latest major revision of the HTML standard. It introduced a wealth of new features, elements, and APIs designed to make web development easier, more powerful, and more semantic. This note highlights some of the most significant additions.

## 1. New Semantic Elements

HTML5 introduced new elements that provide better structure and meaning to web content, improving accessibility and SEO. These replace generic `<div>` elements for common page sections.

*   `<header>`: Introductory content or navigational links.
*   `<nav>`: Navigation links.
*   `<main>`: The dominant content of the `<body>`.
*   `<article>`: Self-contained content (e.g., blog post).
*   `<section>`: Generic standalone section of a document.
*   `<aside>`: Content indirectly related to the main content (e.g., sidebar).
*   `<footer>`: Footer for its nearest sectioning content.
*   `<figure>` and `<figcaption>`: For self-contained content (like images) with a caption.
*   `<mark>`: Highlighted text.
*   `<time>`: Date/time representation.

(See `semantic_html.md` for more details on these.)

## 2. Enhanced Form Controls

HTML5 added new input types and attributes to improve form validation and user experience without relying heavily on JavaScript.

### New Input Types:

*   `color`: Color picker.
*   `date`, `datetime-local`, `month`, `week`, `time`: Date and time controls.
*   `email`: Email address input (basic format validation).
*   `number`: Numeric input (with `min`, `max`, `step` attributes).
*   `range`: Slider control.
*   `search`: Search field.
*   `tel`: Telephone number input.
*   `url`: URL input (basic format validation).

### New Form Attributes:

*   `placeholder`: Provides a hint to the user about what can be entered.
*   `required`: Makes a field mandatory.
*   `autofocus`: Automatically focuses on an input field when the page loads.
*   `pattern`: Specifies a regular expression for input validation.
*   `list`: Connects an input field to a `<datalist>` element for autocomplete suggestions.

```html
<form>
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required placeholder="your@example.com"><br><br>

  <label for="quantity">Quantity (1-5):</label>
  <input type="number" id="quantity" name="quantity" min="1" max="5" value="1"><br><br>

  <label for="search">Search:</label>
  <input type="search" id="search" name="search" autofocus><br><br>

  <label for="browser">Choose your browser from the list:</label>
  <input list="browsers" name="browser" id="browser">
  <datalist id="browsers">
    <option value="Edge">
    <option value="Firefox">
    <option value="Chrome">
    <option value="Opera">
    <option value="Safari">
  </datalist><br><br>

  <input type="submit" value="Submit">
</form>
```

## 3. Media Elements

HTML5 introduced native support for embedding audio and video without requiring third-party plugins like Flash.

### `<audio>`

Embeds audio content.

```html
<audio controls>
  <source src="audio.mp3" type="audio/mpeg">
  <source src="audio.ogg" type="audio/ogg">
  Your browser does not support the audio element.
</audio>
```

### `<video>`

Embeds video content.

```html
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  Your browser does not support the video tag.
</video>
```

**Common attributes for `<audio>` and `<video>`:**

*   `controls`: Displays standard playback controls.
*   `autoplay`: Starts playback automatically (often blocked by browsers).
*   `loop`: Loops the media.
*   `muted`: Mutes the audio.
*   `preload`: Specifies how the browser should load the media.
*   `poster` (for video): Specifies an image to be shown while the video is downloading, or until the user hits the play button.

## 4. Canvas and SVG

*   `<canvas>`: Provides an API for drawing graphics on the fly via JavaScript. Ideal for games, data visualizations, and animations.

    ```html
    <canvas id="myCanvas" width="200" height="100" style="border:1px solid #000000;"></canvas>
    <script>
      var c = document.getElementById("myCanvas");
      var ctx = c.getContext("2d");
      ctx.fillStyle = "#FF0000";
      ctx.fillRect(0,0,150,75);
    </script>
    ```

*   **SVG (Scalable Vector Graphics)**: An XML-based format for describing two-dimensional vector graphics. Can be embedded directly in HTML.

    ```html
    <svg width="100" height="100">
      <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
    </svg>
    ```

## 5. Geolocation API

Allows web applications to access the user's location (with user permission).

```javascript
<script>
function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(showPosition);
  } else {
    alert("Geolocation is not supported by this browser.");
  }
}

function showPosition(position) {
  console.log("Latitude: " + position.coords.latitude + ", Longitude: " + position.coords.longitude);
}
</script>
<button onclick="getLocation()">Get My Location</button>
```

## 6. Web Storage (localStorage and sessionStorage)

Provides a way for web applications to store data locally within the user's browser, offering more capacity and flexibility than cookies.

*   `localStorage`: Stores data with no expiration date.
*   `sessionStorage`: Stores data for one session (data is lost when the browser tab is closed).

```javascript
// localStorage
localStorage.setItem("username", "JohnDoe");
console.log(localStorage.getItem("username")); // "JohnDoe"
localStorage.removeItem("username");

// sessionStorage
sessionStorage.setItem("page_count", "5");
console.log(sessionStorage.getItem("page_count")); // "5"
```

## 7. Web Workers

Allows JavaScript code to run in the background, in a separate thread, without interfering with the user interface. Useful for long-running scripts.

```javascript
// main.js
if (window.Worker) {
  var myWorker = new Worker("worker.js");
  myWorker.onmessage = function(e) {
    console.log('Message received from worker: ' + e.data);
  };
  myWorker.postMessage('Start counting');
}

// worker.js
self.onmessage = function(e) {
  var result = 0;
  for (var i = 0; i < 1000000000; i++) {
    result += i;
  }
  self.postMessage(result);
};
```

HTML5 significantly expanded the capabilities of web browsers, enabling richer, more interactive, and more semantic web applications.