
A Tauri project usually has **2 parts**:  
- **JavaScript project (optional)** – at the top level  
- **Rust project** – inside `src-tauri/`

---
## Typical Structure

```
.
├── package.json
├── index.html
├── src/
│   ├── main.js
├── src-tauri/
│   ├── Cargo.toml
│   ├── Cargo.lock
│   ├── build.rs
│   ├── tauri.conf.json
│   ├── src/
│   │   ├── main.rs
│   │   └── lib.rs
│   ├── icons/
│   │   ├── icon.png
│   │   ├── icon.icns
│   │   └── icon.ico
│   └── capabilities/
│       └── default.json
```

---
## Important Files & Directories

| File/Dir | Purpose |
|----------|----------|
| `tauri.conf.json` | Main Tauri config (app ID, dev server URL, etc). Also used by Tauri CLI to locate the Rust project. |
| `capabilities/` | Defines which commands are allowed to be used in JS code. Default security config folder. |
| `icons/` | Output directory for `tauri icon` command. Referenced in `tauri.conf.json > bundle > icon`. |
| `build.rs` | Contains `tauri_build::build()`, used by Tauri’s build system. |
| `src/lib.rs` | Main Rust code & mobile entry point (`#[cfg_attr(mobile, tauri::mobile_entry_point)]`). Apps are compiled as libraries on mobile. Modify this file instead of `main.rs`. |
| `src/main.rs` | Main entry point for desktop. Runs `tauri_app_lib::run()`. Keep it minimal; don’t modify directly. |

---
