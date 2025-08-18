
## 1. Event System

**Use case:** lightweight pub/sub, cross-window communication, JSON payloads.

### Rust Side (emitters)

```rust
// emit to all windows
app.emit("event-name", Payload { msg: "Hello" })?;

// emit to a specific window
app.emit_to("main", "event-name", Payload { msg: "Hi Main" })?;

// emit with filter
app.emit_filter(
  "event-name",
  Payload { msg: "Hi Filtered" },
  |w| w.label() == "child"
)?;
```

- Payload must implement Clone + Serialize + Send + 'static.
- Errors bubble up as Result<()>.

### Rust Side (listeners)

```rust
app.listen("frontend-event", move |event| {
    println!("Got event {:?}", event.payload());
});

let id = app.once("frontend-event", move |event| {
    println!("Got one-time event: {:?}", event.payload());
});

// stop listening
app.unlisten(id);
```

- `listen`: persists until manually unlistened.
- `once`: auto-deregisters after first call.

### Frontend Side (listeners)

```js
import { listen, once } from "@tauri-apps/api/event";

const unlisten = await listen("event-name", (event) => {
  console.log("Got event", event.payload);
});

await once("event-name", (event) => {
  console.log("Got one-time event", event.payload);
});

// cleanup
unlisten();
```

- `event.payload` is the deserialized JSON object from Rust.

### Notes

- **Cross-window:** events propagate to all windows unless scoped.    
- **Serialization:** always JSON → consider performance when sending large binary data.

---
## Channels

**Use case:** continuous data streams, ordered typed communication.

- Think of channels as **one-to-one pipes** between Rust and frontend.    
- Ideal for file IO, subprocess logs, or long-running tasks.

### Rust Side

```rust
#[tauri::command]
fn stream_logs(channel: tauri::Channel<String>) {
    for i in 0..5 {
        channel.send(format!("Log {}", i)).unwrap();
    }
}
```

- Channels are strongly typed (here: String).
- Closing channel automatically signals EOF to frontend.

### Frontend Side

```js
import { Channel, invoke } from "@tauri-apps/api/core";

const ch = new Channel<string>();

ch.onmessage = (msg) => console.log("Rust sent:", msg);
ch.onclose = () => console.log("Stream closed");

await invoke("stream_logs", { channel: ch });
```

- onmessage: triggered for each message.
- onclose: triggered when Rust side closes channel.

### Notes

- **Single consumer:** only one frontend listener per channel.
- **Performance:** binary data not yet supported → must serialize.
- **Reliability:** channel errors propagate through send.

---
## Eval (JS execution)

**Use case:** ad-hoc JS execution from Rust.

- **Not recommended** for structured communication — mainly useful for triggering small frontend behaviors.

### Rust Side

```rust
window.eval("document.body.style.background = 'red'")?;
```

### Advanced: Structured Data

For sending Rust values directly into JS, use [serialize-to-javascript](https://crates.io/crates/serialize-to-javascript):

```rust
use serialize_to_javascript::ser::to_javascript;
let js_code = format!("handleData({});", to_javascript(&my_struct)?);
window.eval(&js_code)?;
```

### Notes

- **Security risk:** eval executes arbitrary code → sanitize inputs.
- **Performance:** not efficient for frequent updates.

---
## Choosing the Right Method

| **Method**   | **Strengths**                                     | **Weaknesses**                         | **Best Use Cases**                 |
| ------------ | ------------------------------------------------- | -------------------------------------- | ---------------------------------- |
| **Events**   | Global/pub-sub, simple JSON, multi-window support | JSON overhead, not for high throughput | Notifications, state sync          |
| **Channels** | Streaming, strongly typed, backpressure-safe      | One-to-one only, JSON serialization    | Logs, long tasks, progress updates |
| **Eval**     | Direct JS execution, flexible                     | Unsafe, brittle, no type safety        | Quick DOM changes, JS-only interop |

---
## Common Pitfalls

- **Large payloads over events** → serialize to JSON once, avoid cloning large structs.
- **Forgetting to unlisten** → leaks event listeners; always call unlisten().
- **Blocking commands with channels** → if channel blocks on send, spawn it on a new thread.
- **Eval misuse** → never use for user-generated content.

---
