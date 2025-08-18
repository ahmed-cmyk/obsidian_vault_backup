# Modules

ES6 introduced native JavaScript modules, providing a standardized way to organize code into reusable units. Modules allow you to encapsulate code, prevent global namespace pollution, and manage dependencies explicitly.

---
## export

The export keyword is used to make variables, functions, classes, or constants available for use in other modules.

* **Named Exports**: You can export multiple specific members from a module. Consumers must import them by their exact names.

```js
// math.js
export const PI = 3.14159;
export function add(a, b) {
    return a + b;
}
export class Calculator { /* ... */ }
```

* **Default Exports**: A module can have only one default export. This is often used to export the primary entity of a module. When imported, you can give it any name.

```js
// logger.js
const log = (message) => console.log(`[LOG]: ${message}`);
export default log;
```

---
## import

The import keyword is used to bring exported members from other modules into the current module.

* **Named Imports**: Import specific named exports using curly braces `{}`.

```js
// app.js
import { PI, add } from './math.js';
console.log(PI); // 3.14159
console.log(add(2, 3)); // 5
```

* **Default Imports**: Import the default export without curly braces. You can assign it any local name.

```js
// app.js
import myLogger from './logger.js'; // 'myLogger' can be any name
myLogger("Application started.");
```

* **Importing all named exports**: Import all named exports into a single object.

```js
// app.js
import * as MathUtils from './math.js';
console.log(MathUtils.PI);
```

---
## Key Characteristics of Modules

* **Strict Mode by Default**: Module code runs in strict mode automatically.
* **Module Scope**: Variables and functions declared in a module are scoped to that module, not the global scope, unless explicitly exported.
* **Single Evaluation**: A module is evaluated only once, even if imported multiple times. Subsequent imports will use the cached module.
* **Asynchronous Loading**: Modules are loaded asynchronously, preventing blocking of the main thread.
* **Browser Support**: Used with `<script type="module">` tags.
* **Node.js Support**: Supported with `.mjs` file extension or `"type": "module"` in `package.json`.

Modules are essential for building scalable and maintainable JavaScript applications, promoting better organization and dependency management.

---

#javascript