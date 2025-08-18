
Tauri uses a **multi-process architecture** (like Electron or modern browsers) to improve **resilience, security, and performance**.

---
## Why Multiple Processes?

- **Old approach**: Single process → computation + UI + input.  
  - Problem: long tasks freeze UI, crashes kill entire app.  

- **Modern approach**: Isolated processes → safer, resilient apps.  
  - Crashes don’t affect the whole system.  
  - Processes can be restarted independently.  
  - Better CPU utilization (multi-core).  

- **Security**: Apply **Principle of Least Privilege** → each process only has the permissions it needs.  

---
## The Core Process

- **Role**: Application entry point.  
- **Capabilities**: Full OS access.  

- **Responsibilities**:  
  - Create windows, tray menus, notifications.  
  - Orchestrate cross-platform abstractions.  
  - Route all IPC (can filter & intercept centrally).  
  - Manage **global state** (settings, DB connections).  

- **Implementation**: Written in **Rust** for memory safety + performance.  

---
## The WebView Process

- **Role**: Renders UI via OS-provided WebView libraries.  
- **Tech**: HTML, CSS, JS (e.g., Svelte + Vite).  

- **Security practices**:  
  - Sanitize user input.  
  - Don’t handle secrets in Frontend.  
  - Keep business logic in Core → smaller attack surface.  

- **Size benefit**: WebView not bundled, dynamically linked at runtime → smaller binaries.  
- **Caveat**: Must handle platform differences.  

---
## Current WebView Engines

- **Windows** → Microsoft Edge WebView2  
- **macOS** → WKWebView  
- **Linux** → webkitgtk

---