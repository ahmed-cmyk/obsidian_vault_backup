# Streams

Streams are a method for data handling that are used to read, write or transform chunks of data piece by piece **without keeping it in memory all at once**.
They are especially useful for working with large files, network requests, or any data source that cannot (or should not) fit entirely in memory.

---

## Types of Streams

**There are 4 types of streams in NodeJS:**

1. **Readable** — streams from which data can be **read**.
   * Example: `fs.createReadStream()`, `http.IncomingMessage`

2. **Writable** — streams to which data can be **written**.
   * Example: `fs.createWriteStream()`, `http.ServerResponse`

3. **Duplex** — streams that are both **readable and writable**.
   * Example: `net.Socket`

4. **Transform** — a special kind of duplex stream that can **modify** or **transform** the data as it is written and read.
   * Example: `zlib.createGzip()` (compress data while passing it through)

⠀
---

## Key Methods & Events

### Methods

* `.pipe()` — connects a readable stream to a writable stream.
* `.unpipe()` — stops piping.
* `.pause()` / `.resume()` — control flow on readable streams.
* `.write()` — write data to a writable stream.
* `.end()` — signal that no more data will be written.

### Events

| Event | Description |
|---|---|
| `data` | emitted when a chunk of data is available (readable) | 
| `end` | emitted when no more data will be provided (readable) | 
| `error` | emitted on error | 
| `finish` | emitted when all data has been flushed (writable) | 
| `close` | emitted when the stream is closed | 
---

## Example: Reading and Writing with Streams

```js
const fs = require('fs');

const readable = fs.createReadStream('input.txt', { encoding: 'utf8' });
const writable = fs.createWriteStream('output.txt');

readable.pipe(writable);
```

Here we read data from `input.txt` and write it to `output.txt` using `.pipe()` — which handles backpressure automatically.

---

## Backpressure

When the writable stream is slower than the readable stream, **backpressure** happens.
Streams handle this by pausing and resuming the flow of data to avoid overwhelming memory.
Using `.pipe()` automatically manages backpressure for you.

---

## When to Use Streams?

- Reading or writing **large files**
- Streaming **HTTP responses or uploads**
- Compressing/decompressing data
- Processing data that arrives in chunks (like TCP sockets)

---

> **NOTE**: Always handle the `error` event when working with streams to avoid crashes.

---
## Common Mistakes & Best Practices with Streams

### Common Mistakes

* **Not handling `error`events**
  * Streams can emit `error` at any time. If unhandled, it may crash your application.

```js
readable.on('error', err => console.error(err));
writable.on('error', err => console.error(err));
```

* **Forgetting to close streams**
  * Always end writable streams with `.end()` if you’re not using `.pipe()`.
  * Not closing streams may lead to resource leaks.

* **Not managing backpressure**
  * Writing too fast to a slow destination can exhaust memory if you ignore backpressure signals.
  * Use `.pipe()` or check the return value of `.write()` to handle it.

* **Mixing flowing and paused modes carelessly**
  * Calling `.resume()` on a stream switches it to flowing mode, which may conflict with `readable` event listeners.

---

### Best Practices

* Always use `.pipe()` for readable → writable patterns because it manages backpressure and cleanup for you.
* Listen for both `finish` and `error` events when working with writable streams.
* Prefer `createReadStream()` and `createWriteStream()` over reading or writing entire files into memory (`fs.readFile`, `fs.writeFile`).
* If implementing a custom stream, extend the appropriate base class (`Readable`, `Writable`, `Duplex`, or `Transform`) and properly implement `_read()`, `_write()`, etc.
* Use `pipeline()` (from `stream` module) for safer piping in Node.js ≥ v10. This handles cleanup and propagates errors.

### Example: Using `pipeline()` Safely

```js
const { pipeline } = require('stream');
const fs = require('fs');

pipeline(
  fs.createReadStream('input.txt'),
  fs.createWriteStream('output.txt'),
  (err) => {
    if (err) {
      console.error('Pipeline failed:', err);
    } else {
      console.log('Pipeline succeeded.');
    }
  }
);
```

---

#nodejs