# User Specified Errors

User specified errors can be created by extending the base `Error` object, a built-in error class. 

> **NOTE**: When creating errors in this manner, you should pass a message string that describes the error. 

- The message can be accessed through the message property on the object. 
- The Error object also contains a name and a stack property that indicate the name of the error and the point in the code at which it is created.

---
## Error-first callback

```js
function square(num, callback) {
  if (typeof callback !== 'function') {
    throw new TypeError(`Callback must be a function. Got: ${typeof callback}`);
  }

  // simulate async operation
  setTimeout(() => {
    if (typeof num !== 'number') {
      // if an error occurs, it is passed as the first argument to the callback
      callback(new TypeError(`Expected number but got: ${typeof num}`));
      return;
    }

    const result = num * num;
    // callback is invoked after the operation completes with the result
    callback(null, result);
  }, 100);
```

Any caller of this *square* function would need to pass a callback function to access its result or error.

```js
square('8', (err, result) => {
  if (err) {
    console.error(err)
    return
  }

  console.log(result);
});
```

---
## Promise rejections

Promises are the modern way to perform asynchronous operations in Node.js and are now generally preferred to callbacks because this approach has a better flow that matches the way we analyze programs, especially with the `async`/`await` pattern. 

Any Node.js API that utilizes error-first callbacks for asynchronous error handling can be converted to promises using the built-in `util.promisify()` method.

```js
const fs = require('fs');
const util = require('util');

const readFile = util.promisify(fs.readFile);
```

These errors can be caught by chaining a catch method, as show below:

```js
readFile('/path/to/file.txt')
  .then((result) => console.log(result))
  .catch((err) => console.error(err));
```

You can also use promisified APIs in an `async` function, such as the one shown below. This is the predominant way to use promises in modern JavaScript because the code reads like synchronous code, and the familiar `try`/`catch` mechanism can be used to handle errors.

```js
(async function callReadFile() {
  try {
    const result = await readFile('/path/to/file.txt');
    console.log(result);
  } catch (err) {
    console.error(err);
  }
})();
```

---

#nodejs