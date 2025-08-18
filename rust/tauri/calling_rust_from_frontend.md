
## Overview

- **Purpose**: How the web-based frontend communicates with Rust backend using:    
    - Commands: structured and type-safe function calls.
    - Events: flexible, asynchronous, fire-and-forget messages.

---
## Commands – Function-Style IPC

Commands let your frontend invoke Rust functions securely, akin to a foreign-function interface over JSON-RPC.

### 2.1 Basic Example

```rust
#[tauri::command]
fn my_custom_command() {
  println!("Invoked from frontend!");
}

#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
  tauri::Builder::default()
    .invoke_handler(tauri::generate_handler![my_custom_command])
    .run(tauri::generate_context!())
    .expect("error during app run");
}
```

Invoke via frontend:

```js
import { invoke } from '@tauri-apps/api/core';
invoke('my_custom_command');
```

**Notes**:

- Commands in lib.rs **must not** be pub due to naming collision issues in generated code.
- If defined in a separate module (e.g. commands.rs), commands must be pub, but still invoked by their bare name—no module prefix.

### 2.2 Passing Arguments & Returning Data

- Arguments and return values must implement serde::Serialize / serde::Deserialize.
- Example with argument:

```rust
#[tauri::command]
fn echo(message: String) -> String {
  message
}
```

```js
invoke('echo', { message: 'Hello!' }).then(console.log);
```

You can also customize argument naming via attributes like #[tauri::command(rename_all = "snake_case")].

### 2.3 Error Handling

Use Result<T, E> where both T and E are serializable types:

```rust
#[tauri::command]
fn login(user: String, pwd: String) -> Result<String, String> {
  if user == "tauri" && pwd == "secret" { Ok("success".into()) } else { Err("invalid".into()) }
}
```

In JS:

```js
invoke('login', { user: 'tauri', pwd: 'wrong' })
  .then(console.log)
  .catch(console.error);
```

### 2.4 Async Commands

Define async commands to perform non-blocking work efficiently.

```rust
#[tauri::command]
async fn heavy_task(input: String) -> String {
  // heavy processing here
  input.to_uppercase()
}
```

> **Caution**: Borrowed arguments like &str or State<'_, Data> aren’t supported. Use owned types (String) or wrap in Result.

### 2.5 Channels (Streaming)

Ideal for sending streaming data chunks:

```rust
#[tauri::command]
async fn load_file(path: std::path::PathBuf, reader: tauri::ipc::Channel<&[u8]>) {
  // stream data in chunks to reader...
}
```

Great for downloads or long-running operations.

### 2.6 Accessing Context

Commands can access:

- WebviewWindow: identify source window.    
- AppHandle: access application context (e.g., shortcuts).
- `State<T>`: access managed state within the app.
- IPC::Request: raw request payload and headers.

### 2.7 Registering Multiple Commands

```
invoke_handler(tauri::generate_handler![cmd1, cmd2, cmd3]);
```

> **NOTE:** All commands must be in a single macro invocation.

---
## Event System – Lightweight Messaging

Events are untyped, async messages best suited for state or lifecycle updates.

### 3.1 Emitting Events (Rust)

- **Global event**:

```rust
app.emit("update-ready", &version).unwrap();
```

- **To specific window**:

```rust
app.emit_to("settings", "config-changed", config).unwrap();
```

### 3.2 Listening (Frontend)

```js
import { listen } from '@tauri-apps/api/event';
const unlisten = await listen('update-ready', e => {
  console.log(e.payload);
});
// Cleanup via unlisten()
```

> **NOTE:** Events are delivered as JSON; use listen_any to catch across windows.

---
## IPC Design Recap

| **Mechanism** | **Direction**    | **Use Case**                  | **Type Safety** |
| ------------- | ---------------- | ----------------------------- | --------------- |
| Commands      | Frontend → Rust  | Structured calls & responses  | Strong (JSON)   |
| Events        | One/Bi-direction | Notifications, status changes | Weak (JSON)     |

This follows an Asynchronous Message Passing model, offering safety over direct FFI.

---
