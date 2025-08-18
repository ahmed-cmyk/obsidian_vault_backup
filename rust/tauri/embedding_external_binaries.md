
## What is a Sidecar?

A **sidecar** is an external binary bundled with your Tauri app.

Typical use cases:

- Ship a Python CLI tool compiled with pyinstaller.    
- Bundle an existing API server or helper executable.
- Avoid requiring end users to install extra dependencies (Node.js, Python, etc.).

---
## 1. Adding Sidecars to the Bundle

**Define binaries in `src-tauri/tauri.conf.json`:**

```json
{
  "bundle": {
    "externalBin": [
      "/absolute/path/to/sidecar",
      "../relative/path/to/binary",
      "binaries/my-sidecar"
    ]
  }
}
```

- Paths are relative to src-tauri/tauri.conf.json.

*Example: `binaries/my-sidecar` →` <PROJECT ROOT>/src-tauri/binaries/my-sidecar`*

### Platform-Specific Binaries

Each binary must exist with a **target triple suffix** for every architecture you ship for.

Example (externalBin: ["binaries/my-sidecar"]):

- Linux: src-tauri/binaries/my-sidecar-x86_64-unknown-linux-gnu
- macOS ARM: src-tauri/binaries/my-sidecar-aarch64-apple-darwin
- Windows: src-tauri/binaries/my-sidecar-x86_64-pc-windows-msvc.exe

**Get your target triple:**

```bash
rustc -Vv | grep host | cut -f2 -d' '
```

**Or in PowerShell (Windows):**

```powershell
rustc -Vv | Select-String "host:" | ForEach-Object {$_.Line.split(" ")[1]}
```

### Auto-Renaming Example

**Node.js script to rename sidecar for the current host:**

```js
import { execSync } from 'child_process';
import fs from 'fs';

const extension = process.platform === 'win32' ? '.exe' : '';
const rustInfo = execSync('rustc -vV');
const targetTriple = /host: (\S+)/g.exec(rustInfo)[1];

fs.renameSync(
  `src-tauri/binaries/sidecar${extension}`,
  `src-tauri/binaries/sidecar-${targetTriple}${extension}`
);
```

⚠️ Only works for **native builds** (not cross-compilation).

---
## 2. Running Sidecars from Rust

**Requires the [plugin-shell](https://v2.tauri.app/plugin/shell/) plugin.**

```rust
use tauri_plugin_shell::ShellExt;
use tauri_plugin_shell::process::CommandEvent;

#[tauri::command]
async fn run_sidecar(app: tauri::AppHandle) {
  let sidecar_command = app.shell().sidecar("my-sidecar").unwrap();
  let (mut rx, mut child) = sidecar_command.spawn().expect("Failed to spawn");

  tauri::async_runtime::spawn(async move {
    while let Some(event) = rx.recv().await {
      if let CommandEvent::Stdout(line_bytes) = event {
        let line = String::from_utf8_lossy(&line_bytes);
        println!("Sidecar said: {}", line);

        // Optionally write back
        child.write("Hello from Rust\n".as_bytes()).unwrap();
      }
    }
  });
}
```

👉 sidecar("my-sidecar") takes the binary **name**, not the path.

---
## 3. Running Sidecars from JavaScript

**Grant permissions in `src-tauri/capabilities/default.json`:**

```json
{
  "permissions": [
    "core:default",
    {
      "identifier": "shell:allow-execute",
      "allow": [
        {
          "name": "binaries/my-sidecar",
          "sidecar": true
        }
      ]
    }
  ]
}
```

**JS Code:**

```js
import { Command } from '@tauri-apps/plugin-shell';

const command = Command.sidecar('binaries/my-sidecar');
const output = await command.execute();

console.log(output.code);   // exit code
console.log(output.stdout); // captured stdout
console.log(output.stderr); // captured stderr
```

---
## 4. Passing Arguments

**Define allowed arguments in `capabilities/default.json`:**

```json
{
  "identifier": "shell:allow-execute",
  "allow": [
    {
      "name": "binaries/my-sidecar",
      "sidecar": true,
      "args": [
        "arg1",
        "-a",
        "--arg2",
        { "validator": "\\S+" }
      ]
    }
  ]
}
```

**Rust:**

```rust
let sidecar_command = app
  .shell()
  .sidecar("my-sidecar").unwrap()
  .args(["arg1", "-a", "--arg2", "some-value"]);

let (_rx, _child) = sidecar_command.spawn().unwrap();
```

**JavaScript:**

```js
const command = Command.sidecar('binaries/my-sidecar', [
  'arg1', '-a', '--arg2', 'some-value'
]);
const output = await command.execute();
```

✅ **Key Takeaways**

- Sidecars let you bundle arbitrary executables.    
- Name resolution depends on target triple.
- Use plugin-shell for spawning, executing, or streaming IO.
- Permissions (capabilities/default.json) **must** be configured to control what sidecars can run and with what arguments.

---
