# Event Emitter

In Node.js, an event can be described simply as a string with a corresponding callback. An event can be "emitted" (or, in other words, the corresponding callback be called) multiple times or you can choose to only listen for the first time it is emitted.

**Initialization:**

```js
import EventEmitter from 'node:events';

const eventEmitter = new EventEmitter();
```

This object exposes among many others, the `on` and `emit` methods.

- `emit` is used to trigger an event
- `on` is used to add a callback function that’s going to be executed when the event is triggered.

You can pass arguments to the event handler by passing them as additional arguments to `emit()`:

```js
eventEmitter.on('start', number => {
  console.log(`started ${number}`);
});

eventEmitter.emit('start', 23);
```

Multiple arguments:

```js
eventEmitter.on('start', (start, end) => {
  console.log(`started from ${start} to ${end}`);
});

eventEmitter.emit('start', 1, 100);
```

The EventEmitter object also exposes several other methods to interact with events, like

* `once()`: add a one-time listener
* `removeListener()` / `off()`: remove an event listener from an event
* `removeAllListeners()`: remove all listeners for an event

---

#nodejs