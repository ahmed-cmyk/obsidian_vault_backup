
## Commands

- **What they are:** Functions written in Rust, exposed to the frontend via `#[tauri::command]`.
- **Scope:** App-specific (live inside your project).
- **Best for:**
  - Small apps or prototypes.
  - Simple business logic or utilities.
  - Features unlikely to be reused outside this app.
  - Quick bridging between frontend & Rust.

**Example:**  

```rust
#[tauri::command]
fn greet(name: String) -> String {
  format!("Hello, {}!", name)
}
```

---
## **Plugins**

- **What they are:** Modular packages (Rust crates) that extend Tauri functionality.
- **Scope:** Reusable across multiple apps. Can be published and shared.
- **Best for:**
    - Complex functionality (filesystem, updater, auth, etc.).
    - Features needed in **multiple apps**.
    - When you want clean separation of concerns.
    - Community-driven features (installable from ecosystem).

**Example:**
- Official: tauri-plugin-fs, tauri-plugin-http.    
- Custom: Your own auth plugin for multiple internal apps.

---
## **Comparison**

| **Aspect**       | **Commands**                 | **Plugins**                            |
| ---------------- | ---------------------------- | -------------------------------------- |
| **Scope**        | Project-only                 | Reusable across projects               |
| **Complexity**   | Simple                       | More structured / modular              |
| **Use Case**     | Quick features, app-specific | Complex, shared, or ecosystem features |
| **Distribution** | Lives in your app codebase   | Can be published & shared as a crate   |

---
## **Rule of Thumb**

- Start with **Commands** when experimenting or building small features.    
- Extract into a **Plugin** if:
    - The feature grows too large.
    - You want to reuse it in multiple apps.
    - You want to share it with the Tauri ecosystem.

---
