# Browser & Environment

Understanding the browser environment is crucial for front-end JavaScript development, as it provides the APIs and context for web applications.

---
## Difference between DOM & BOM

The **Document Object Model (DOM)** and the **Browser Object Model (BOM)** are two distinct but related sets of APIs that JavaScript interacts with in a web browser.

### Document Object Model (DOM)

* Represents the **structure of an HTML or XML document** as a tree-like model.
* It provides an API (methods and properties) for JavaScript to **access and manipulate the content, structure, and style of web pages**.
* The root of the DOM is the `document` object.
* **Examples of DOM APIs**: `document.getElementById()`, `document.createElement()`, `element.addEventListener()`, `element.style.color`, `appendChild()`.
* The DOM is a **W3C standard**, meaning its specifications are consistent across different browsers.

```js
// DOM example
document.body.style.backgroundColor = "lightblue";
const newDiv = document.createElement("div");
newDiv.textContent = "Hello DOM!";
document.body.appendChild(newDiv);
```

### Browser Object Model (BOM)

* Represents the **browser window and its surrounding environment**, not the content of the document itself.
* It provides objects and methods to interact with the browser window, history, location, screen, etc.
* The root of the BOM is the `window` object, which also serves as the global object in browsers.
* **Examples of BOM APIs**: `window.alert()`, `window.location`, `window.history`, `window.navigator`, `setTimeout()`, `setInterval()`.
* The BOM is **not a formal W3C standard** in its entirety (though parts like `window.location` are standardized). This means there can be slight inconsistencies in BOM implementations across different browsers.

```js
// BOM example
alert("Welcome to the page!");
console.log(window.location.href); // Current URL
window.open("https://www.example.com", "_blank"); // Open a new tab
```

> **NOTE**: In essence, the DOM deals with the *content of the page*, while the BOM deals with the *browser window itself*.

---
## What are `document` & `window`?

`document` and `window` are two of the most important global objects available in the browser environment, serving as the entry points to the DOM and BOM, respectively.

### `window`object

* The **global object** in web browsers. All global variables, functions, and objects defined in the browser's JavaScript environment (including `document`) are properties of the `window` object.
* It represents the **browser window or tab** itself.
* It provides access to BOM functionalities (e.g., `window.location`, `window.history`, `window.navigator`, `setTimeout`, `alert`).
* When you write `alert()`, you are implicitly calling `window.alert()`.

```js
console.log(window); // The global window object
console.log(window.innerWidth); // Width of the viewport
window.scrollTo(0, 0); // Scroll to top
```

### `document`object

* A property of the `window` object (`window.document`).
* It represents the **Document Object Model (DOM)** of the current web page.
* It provides the API to interact with the HTML/XML content of the page. You use the `document` object to select elements, create new elements, modify content, handle events, and change styles.

```js
console.log(document); // The document object representing the HTML page
const title = document.title; // Get page title
document.body.innerHTML = "<h1>New Content</h1>"; // Change body content
```

In summary, `window` is the overarching global object for the browser environment, while `document` is a specific property of `window` that provides access to the web page's content and structure via the DOM.

---
## Event bubbling, capturing & delegation

Event handling in the DOM involves three phases: capturing, at target, and bubbling. Understanding these phases is crucial for effective event management, especially for event delegation.

### Event Capturing Phase

* The event starts from the `window` object (or the root of the document) and propagates **downwards** through the DOM tree to the target element.
* During this phase, event listeners registered with `useCapture = true` (or `{ capture: true }` in modern `addEventListener`) will be triggered.
* This phase is rarely used in practice compared to bubbling, but it can be useful for intercepting events before they reach the target.

```html
<!-- HTML Structure -->
<div id="grandparent">
    <div id="parent">
        <button id="child">Click Me</button>
    </div>
</div>
```

```js
const grandparent = document.getElementById('grandparent');
const parent = document.getElementById('parent');
const child = document.getElementById('child');

grandparent.addEventListener('click', () => console.log('Grandparent (Capturing)'), { capture: true });
parent.addEventListener('click', () => console.log('Parent (Capturing)'), { capture: true });
child.addEventListener('click', () => console.log('Child (Target)')); // Target phase
```

### At Target Phase

Once the event reaches the actual target element that triggered it, the event listeners directly attached to that element are executed.

### Event Bubbling Phase

* After the event reaches the target element and its listeners are executed, the event then propagates **upwards** from the target element back to the `window` object.
* During this phase, event listeners registered with `useCapture = false` (the default for `addEventListener`) will be triggered on parent elements.
* This is the most commonly used phase for event handling.

```js
// ... (from above)
grandparent.addEventListener('click', () => console.log('Grandparent (Bubbling)'));
parent.addEventListener('click', () => console.log('Parent (Bubbling)'));

// If you click the 'child' button, the order of console logs would be:
// Grandparent (Capturing)
// Parent (Capturing)
// Child (Target)
// Parent (Bubbling)
// Grandparent (Bubbling)
```

### Event Delegation

Event delegation is a powerful technique that leverages event bubbling. Instead of attaching individual event listeners to many child elements, you attach a single event listener to a common parent element. When an event bubbles up to the parent, you can then determine which specific child element was the original target of the event (using event.target) and handle it accordingly.

**Benefits of Event Delegation**:

* **Performance**: Reduces the number of event listeners, especially for large lists or tables, leading to less memory consumption and better performance.
* **Dynamic Elements**: Automatically handles events for elements that are added to the DOM *after* the initial page load, without needing to reattach listeners.
* **Simpler Code**: Makes code cleaner and easier to manage.


```js
const list = document.getElementById('myList'); // A parent <ul>

list.addEventListener('click', function(event) {
    // Check if the clicked element is an <li> item
    if (event.target.tagName === 'LI') {
        console.log('Clicked item:', event.target.textContent);
        event.target.style.backgroundColor = 'yellow';
    }
});

// Even if new <li> items are added later, this single listener will handle them.
```

---
## Storage options: `localStorage`, `sessionStorage`, cookies

Web browsers provide several mechanisms for storing data on the client-side, each with different characteristics regarding persistence, scope, and capacity.

### `localStorage`

* **Persistence**: Data persists even after the browser window is closed and reopened. It has no expiration date unless explicitly cleared by the user, by the application, or by browser settings.
* **Scope**: Data is accessible across all tabs and windows from the same origin (same protocol, host, and port).
* **Capacity**: Typically around 5-10 MB.
* **API**: Simple key-value store. Values are stored as strings.

  * `localStorage.setItem(key, value)`: Stores a key-value pair.
  * `localStorage.getItem(key)`: Retrieves a value.
  * `localStorage.removeItem(key)`: Removes a key-value pair.
  * `localStorage.clear()`: Clears all data for the origin.

* **Use Cases**: Storing user preferences, offline data, client-side caching.

```js
localStorage.setItem('username', 'Alice');
const user = localStorage.getItem('username');
console.log(user); // Alice
```

### `sessionStorage`

* **Persistence**: Data is stored only for the duration of the **session**. It is cleared when the browser tab or window is closed. Data persists across page reloads and restores within the same tab.
* **Scope**: Data is isolated to the specific tab or window where it was created. It is not shared across different tabs/windows, even from the same origin.
* **Capacity**: Similar to `localStorage`, typically around 5-10 MB.
* **API**: Same key-value API as `localStorage`.
* **Use Cases**: Storing temporary data relevant to a single session, like form data across multi-step forms, or temporary user input.

```js
sessionStorage.setItem('currentStep', '3');
const step = sessionStorage.getItem('currentStep');
console.log(step); // 3
```

### Cookies

* **Persistence**: Can be set with an expiration date (persistent) or be session-based (cleared when browser closes).
* **Scope**: Sent with *every* HTTP request to the server for the domain they are set for. Can be set for specific paths.
* **Capacity**: Very small, typically around 4 KB per domain. Limited number of cookies per domain (e.g., 20-50).
* **API**: Accessed via `document.cookie` (string manipulation required, not a direct key-value map). Server-side can also set cookies via HTTP headers.
* **Use Cases**: Session management (e.g., user authentication), tracking user behavior, small user preferences.
* **Security**: Can be vulnerable to XSS attacks if not handled carefully (use `HttpOnly` flag). Can be set with `Secure` flag for HTTPS only.

```js
// Setting a cookie (manual string formatting)
document.cookie = "myCookie=value; expires=Thu, 18 Dec 2025 12:00:00 UTC; path=/";

// Getting cookies (requires parsing the string)
console.log(document.cookie); // "myCookie=value; anotherCookie=anotherValue"
```

---
## CORS & same-origin policy

**CORS (Cross-Origin Resource Sharing)** and the **Same-Origin Policy (SOP)** are fundamental security mechanisms in web browsers that govern how web pages can interact with resources (like APIs, images, fonts) loaded from different origins.

### Same-Origin Policy (SOP)

* The SOP is a critical security concept that restricts how a document or script loaded from one origin can interact with a resource from another origin.
* **Origin** is defined by the combination of **protocol (scheme)**, **host (domain)**, and **port**. All three must match for two URLs to be considered "same-origin."

  * `http://example.com:80/path` is different from `https://example.com:80/path` (protocol differs).
  * `http://example.com:80/path` is different from `http://sub.example.com:80/path` (host differs).
  * `http://example.com:80/path` is different from `http://example.com:8080/path` (port differs).

* **Restriction**: By default, the SOP prevents JavaScript running on a web page from making **XMLHttpRequest (XHR)** or **Fetch API** requests to a different origin. This prevents malicious scripts from one site from reading sensitive data from another site (e.g., your banking site) without your permission.
* **Exceptions**: Certain HTML tags (like `<img>`, `<script>`, `<link>`, `<video>`) are generally allowed to load cross-origin resources, but JavaScript cannot read their content directly.

### CORS (Cross-Origin Resource Sharing)

* CORS is a mechanism that allows web servers to explicitly grant permission for web pages from different origins to access their resources. It's an extension of the SOP.
* When a browser detects a cross-origin request (e.g., a `fetch` request from `site-a.com` to `api.site-b.com`), it adds an `Origin` header to the request.
* The server at `api.site-b.com` must respond with specific **CORS headers** (most importantly, `Access-Control-Allow-Origin`) to indicate to the browser that it's safe to allow the requesting origin to access the resource.
* If the server does *not* send the appropriate CORS headers, the browser will block the response from being accessible to the JavaScript code, resulting in a CORS error in the console.

#### Types of CORS Requests

* **Simple Requests**: Meet certain criteria (e.g., using `GET`, `HEAD`, `POST` methods, specific content types). The browser sends the request directly with an `Origin` header.
* **Preflighted Requests**: For more complex requests (e.g., using `PUT`, `DELETE`, custom headers, non-standard content types), the browser first sends an `OPTIONS` "preflight" request to the server. This preflight asks the server for permission to send the actual request. Only if the preflight is successful (server responds with appropriate CORS headers) will the browser send the actual request.

> **NOTE**: CORS is a server-side configuration that enables controlled cross-origin communication, while SOP is a browser-enforced security policy that restricts it by default.

---
## How `fetch` works & handling errors

The **Fetch API** provides a modern, Promise-based interface for making network requests (like HTTP requests) in web browsers. It's designed to replace the older `XMLHttpRequest` (XHR) API, offering a more powerful and flexible way to fetch resources.

### How fetch works

`fetch()` takes at least one argument: the URL of the resource to fetch. It returns a `Promise` that resolves to the `Response` object representing the response to the request.

```js
fetch('[https://api.example.com/data](https://api.example.com/data)')
    .then(response => {
        // The first .then() receives the Response object.
        // Check if the response was successful (status 200-299)
        if (!response.ok) {
            // Throw an error for HTTP error statuses (e.g., 404, 500)
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        // Parse the response body as JSON
        return response.json();
    })
    .then(data => {
        // The second .then() receives the parsed JSON data
        console.log('Data:', data);
    })
    .catch(error => {
        // .catch() handles network errors or errors thrown in .then() blocks
        console.error('Fetch error:', error);
    });
```

### Key points about `fetch`

* **Promise-based**: Returns a Promise, allowing for easy chaining with `.then()`, `.catch()`, and `.finally()`.
* **`Response`object**: The Promise resolves with a `Response` object, which contains information about the response (headers, status, etc.). It also provides methods to parse the response body (e.g., `response.json()`, `response.text()`, `response.blob()`).
* **Network errors only for `catch`**: `fetch` only `rejects` its Promise if a **network error** occurs (e.g., no internet connection, CORS issue). It does *not* reject for HTTP error statuses like 404 (Not Found) or 500 (Internal Server Error). For these, the Promise still resolves, but `response.ok` will be `false`.

### Handling Errors with `fetch`

Due to `fetch`'s behavior of not rejecting on HTTP error statuses, you must explicitly check `response.ok` (which is `true` for 2xx status codes) or `response.status` in the first `.then()` block. If `response.ok` is `false`, you should manually throw an `Error` to propagate it down the Promise chain to a `.catch()` block.

```js
async function fetchData(url) {
    try {
        const response = await fetch(url);

        // Check for HTTP errors (e.g., 404, 500)
        if (!response.ok) {
            const errorText = await response.text(); // Get error message from body
            throw new Error(`HTTP error! status: ${response.status}, message: ${errorText}`);
        }

        const data = await response.json();
        return data;
    } catch (error) {
        // This catch block handles:
        // 1. Network errors (e.g., no internet, CORS)
        // 2. Errors explicitly thrown from the 'try' block (e.g., !response.ok)
        // 3. Errors during JSON parsing (if response.json() fails)
        console.error("Error fetching data:", error.message);
        throw error; // Re-throw to propagate the error further if needed
    }
}

fetchData('[https://api.example.com/nonexistent-endpoint](https://api.example.com/nonexistent-endpoint)')
    .catch(err => console.error("Caught error outside async function:", err.message));
```

Using `async/await` with `try...catch` provides a clean and readable way to manage both network and HTTP status errors with `fetch`.

---

#javascript