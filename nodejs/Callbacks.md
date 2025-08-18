# Callbacks

> **NOTE**: Being an asynchronous platform, NodeJS doesn’t wait around for things like file I/O to finish — NodeJS uses callbacks.

A `callback` is a function called at the completion of a given task; this prevents any blocking, and allows other code to be run in the meantime.

```js
function greet(name) {
  console.log(`Hello, ${name}!`);
}

// Pass 'greet' as a callback
setTimeout(() => {
  greet("Ahmed");
}, 1000);

console.log("Waiting to greet...");

```

---

#nodejs