
Inter-process communication (IPC) allows isolated processes to communicate securely and is key to building more complex applications.

> **NOTE:** Tauri uses a particular style of Inter-Process Communication called [Asynchronous Message Passing](https://en.wikipedia.org/wiki/Message_passing#Asynchronous_message_passing), where processes exchange _requests_ and _responses_ serialized using some simple data representation. Message Passing should sound familiar to anyone with web development experience, as this paradigm is used for client-server communication on the internet.

Message passing is a safer technique than shared memory or direct function access because the recipient is free to reject or discard requests as it sees fit.

---
## Commands

Tauri provides an **FFI-like abstraction** on top of IPC messages through the `invoke` API.

- **`invoke`** (similar to browser’s `fetch`)  
  - Frontend can call Rust functions, pass arguments, and receive return data.  
- Works like **FFI (Foreign Function Interface)** but safer:  
  - FFI = direct language bindings (e.g., C ↔ Rust), risky due to memory corruption and unsafe pointer usage.  
  - Tauri avoids this by using **message passing** under the hood.  

> **Note**: Commands are not real FFI, so they do not share the same security pitfalls.

### JSON-RPC in Tauri
- Under the hood, Tauri uses a **JSON-RPC–like protocol** for serializing requests and responses.  
- All arguments and return data must be **serializable to JSON**.  

#### Benefits
- Simple and widely understood.  
- Language-agnostic (any language can parse JSON).  
- Easy to debug (plain text).  

#### Limitations
- **Serialization overhead** → slower than binary protocols.  
- **Limited type support** → only JSON-compatible data (no BigInts, Dates, complex objects).  
- **Large payloads** → inefficient for files or binary data.  
- **Error handling** → standardized but less expressive for complex Rust errors.  
- **Security** → JSON is text-based, so unsanitized input risks injection.  

### Comparison: JSON-RPC vs FFI vs Binary IPC

| Aspect           | JSON-RPC (Tauri)                                | Real FFI (e.g., C ↔ Rust)                | Binary IPC (e.g., Protocol Buffers, Cap’n Proto) |
| ---------------- | ----------------------------------------------- | ---------------------------------------- | ------------------------------------------------ |
| **Safety**       | High (message passing, no direct memory access) | Low (memory corruption, unsafe pointers) | High (strong schema, isolated processes)         |
| **Ease of Use**  | Very easy (plain JSON, debug-friendly)          | Complex (requires bindings, unsafe code) | Medium (need schema definitions, tooling)        |
| **Performance**  | Moderate (serialization overhead)               | Very fast (direct calls)                 | High (efficient binary encoding)                 |
| **Type Support** | Limited (JSON types only)                       | Full (all native types)                  | Rich (custom types, enums, nested structures)    |
| **Portability**  | Excellent (works everywhere)                    | Limited (depends on platform/compiler)   | Good (needs compatible tooling)                  |

---
## Events

Events are fire-and-forget, one way IPC messages that are best suited to communicate lifestyle events and state changes. Unlike Commands, Events can be emitted by both the Frontend and the tauri core.

---
## Patterns

### Brownfield

**Default pattern** for Tauri apps.

- Simplest & most straightforward way to integrate Tauri.  
- Aims for **maximum compatibility** with existing frontend projects.  
- Requires nothing beyond what a browser-based frontend already uses.  
- **Limitation**: Not everything from browser apps works out-of-the-box.  

#### Concept

- **Brownfield** = building on top of existing systems.  
- For Tauri: “existing system” = current browser support & behavior.  

#### Configuration

- **Default** → no config needed.  
- To explicitly set in `tauri.conf.json`:  

```json
{
  "app": {
    "security": {
      "pattern": {
        "use": "brownfield"
      }
    }
  }
}
```

### Isolation

**Most secure pattern** for Tauri apps. 

- Provides **strict separation** between the Webview (frontend) and Rust (backend).  
- No direct access to the system from the Webview → everything must go through explicitly defined APIs.  
- Prevents loading arbitrary remote content directly into the app (mitigates XSS / injection risks).  
- Recommended for **apps that deal with sensitive data** or need hardened security.  

#### Concept

- The frontend is treated as **untrusted content**.  
- The backend defines **capabilities** and only allows explicitly approved actions.  
- Inspired by **web sandboxing** and **zero-trust principles**.  

#### Configuration

Set in `tauri.conf.json`:  

```json
{
  "app": {
    "security": {
      "pattern": {
        "use": "isolation"
      }
    }
  }
}
```

---