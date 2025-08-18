
Tauri apps often need global/shared state, such as counters, config, or long-lived resources.

Tauri provides **Manager** and the **State** API to register and access state.

---
## 1. Defining and Registering State

You register state during .setup with `app.manage()`.

```rust
use tauri::{Builder, Manager};

struct AppData {
  welcome_message: &'static str,
}

fn main() {
  Builder::default()
    .setup(|app| {
      app.manage(AppData {
        welcome_message: "Welcome to Tauri!",
      });
      Ok(())
    })
    .run(tauri::generate_context!())
    .unwrap();
}
```

Later you can retrieve it with:

```rust
let data = app.state::<AppData>();
println!("{}", data.welcome_message);
```

---
## 2. Mutability and Interior Mutability

- **Problem**: State is shared across threads → direct mutation is unsafe.
- **Solution**: Wrap your state in Mutex, RwLock, or an async lock.

```rust
use std::sync::Mutex;
use tauri::{Builder, Manager};

#[derive(Default)]
struct AppState {
  counter: u32,
}

fn main() {
  Builder::default()
    .setup(|app| {
      app.manage(Mutex::new(AppState::default()));
      Ok(())
    })
    .run(tauri::generate_context!())
    .unwrap();
}
```

Usage:

```rust
let state = app.state::<Mutex<AppState>>();
let mut state = state.lock().unwrap();
state.counter += 1;
```

---
## 3. Async Mutex

- Use `std::sync::Mutex` for most cases (even in async code).
- Use `tokio::sync::Mutex` if you must hold a lock **across await points**.

```rust
#[tauri::command]
async fn increase_counter(state: State<'_, tokio::sync::Mutex<AppState>>) -> u32 {
  let mut state = state.lock().await;
  state.counter += 1;
  state.counter
}
```

---
## 4. Accessing State in Commands

```rust
use tauri::State;
use std::sync::Mutex;

#[derive(Default)]
struct AppState {
  counter: u32,
}

#[tauri::command]
fn increase_counter(state: State<'_, Mutex<AppState>>) -> u32 {
  let mut state = state.lock().unwrap();
  state.counter += 1;
  state.counter
}
```

---
## 5. Accessing State Outside Commands (Manager API)

For event handlers, threads, or background tasks, use the **Manager** trait:

```rust
use std::sync::Mutex;
use tauri::{Builder, Manager, Window, WindowEvent};

#[derive(Default)]
struct AppState {
  counter: u32,
}

fn on_window_event(window: &Window, _event: &WindowEvent) {
    let app_handle = window.app_handle();
    let state = app_handle.state::<Mutex<AppState>>();

    let mut state = state.lock().unwrap();
    state.counter += 1;
}

fn main() {
  Builder::default()
    .setup(|app| {
      app.manage(Mutex::new(AppState::default()));
      Ok(())
    })
    .on_window_event(on_window_event)
    .run(tauri::generate_context!())
    .unwrap();
}
```

---
## 6. Common Pitfalls

- ❌ **Wrong type → runtime panic**
    If you register Mutex`<AppState>` but try State`<'_, AppState>`, you’ll panic at runtime.

✅ **Use a type alias** to avoid confusion:

```rust
use std::sync::Mutex;

#[derive(Default)]
struct AppStateInner {
  counter: u32,
}

type AppState = Mutex<AppStateInner>; // Always use this alias
```

---
## 7. When to Use Arc?

- You **don’t need Arc** with Tauri State — it’s already wrapped.
- Only use `Arc<Mutex<T>>` yourself if you manually spawn threads outside Tauri’s context.

---
## Quick Use Cases

- Global config / theme data.    
- Database pool (`Arc<Mutex<Pool>>`).
- Background job counters.
- Long-lived service handles (e.g., WebSocket client).

---
