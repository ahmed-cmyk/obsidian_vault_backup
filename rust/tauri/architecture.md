
## Introduction
- **Toolkit**: Polyglot, composable, used for desktop apps.  
- **Tech stack**: Rust (backend) + HTML/Webview (frontend).  
- **APIs**: Optional JS API & Rust API via message passing.  
- **Extensibility**: Easily bridge Webview ↔ Rust backend.  

### Key Features
- Supports tray-type interfaces.  
- Apps are **small** (use OS’s webview, no runtime shipped).  
- Final binary compiled from Rust → harder to reverse engineer.  
- Updates managed by the OS.  

---
## What Tauri is *Not*
- ❌ Not a kernel wrapper → uses **WRY** + **TAO** for system calls.  
- ❌ Not a VM → it’s a toolkit for making Webview OS apps.  

---
## Core Ecosystem

| Crate                 | Purpose                                                                                                             |
| --------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **tauri**             | Main crate; config, runtime, macros, API, updates. Reads `tauri.conf.json`. Handles script injection & system APIs. |
| **tauri-runtime**     | Glue layer between Tauri & lower-level webview libs.                                                                |
| **tauri-macros**      | Provides macros for context, handler, and commands.                                                                 |
| **tauri-utils**       | Utilities: parse config, detect platforms, inject CSP, manage assets.                                               |
| **tauri-build**       | Applies macros at build-time; enables cargo features.                                                               |
| **tauri-codegen**     | Embeds, hashes, compresses assets (e.g., icons, tray). Parses config at compile time.                               |
| **tauri-runtime-wry** | System-level interactions with WRY (printing, monitor detection, windowing).                                        |

---
## Tauri Tooling

| Tool | Description |
|------|-------------|
| **API (JS/TS)** | Provides JS/TS endpoints for frontend → backend communication. Ships as TS, CJS, and ESM. |
| **Bundler (Rust/Shell)** | Builds Tauri apps for macOS, Windows, Linux (mobile coming). Usable outside Tauri projects. |
| **cli.rs (Rust)** | Full CLI interface, runs cross-platform. |
| **cli.js (JavaScript)** | NPM wrapper around `cli.rs` using napi-rs. |
| **create-tauri-app (JS)** | Rapid scaffolding of Tauri projects with chosen frontend framework. |

---
## Upstream Crates

| Crate | Purpose |
|-------|---------|
| **TAO** | Window management. Cross-platform (Win/Mac/Linux/iOS/Android). Fork of `winit`, extended for menus, tray, etc. |
| **WRY** | Webview rendering library (Win/Mac/Linux). Tauri uses it to abstract which webview is used & handle interactions. |

---
## Additional Tooling

- **tauri-action** → GitHub workflow to build binaries across platforms.  
- **tauri-vscode** → VS Code extension with Tauri support.  
- **vue-cli-plugin-tauri** → Quick Tauri integration in Vue CLI projects.  

---
## Plugins
- Plugins extend functionality with Rust + JS glue code.  
- Structure:  
  1. Rust code for functionality  
  2. Glue layer for integration  
  3. JS API for frontend  

### Examples
- `tauri-plugin-fs`  
- `tauri-plugin-sql`  
- `tauri-plugin-stronghold`  

---
