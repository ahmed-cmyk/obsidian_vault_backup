
You may need to include additional files in your application bundle that aren't part of your frontend (your `frontendDist`) directly or which are too big to be inlined into the binary. We call these files `resources`.

To bundle the files of your choice, you can add the `resource` property to the `bundle` object in your `tauri.conf.json` file.

> **NOTE:** `resources` expects a list of strings targeting files or directories either with absolute or relative paths. It supports glob patterns in case you need to include multiple files from a directory.

```json
// tauri.conf.json
{
  "bundle": {
    "resources": [
      "/absolute/path/to/textfile.txt",
      "relative/path/to/jsonfile.json",
      "resources/**/*"
    ]
  }
}
```

Alternatively the `resources` config also accepts a map object if you want to change where the files will be copied to. Here is a sample that shows how to include files from different sources into the same `resources` folder:

```json
// tauri.conf.json
{
  "bundle": {
    "resources": {
      "/absolute/path/to/textfile.txt": "resources/textfile.txt",
      "relative/path/to/jsonfile.json": "resources/jsonfile.json",
      "resources/**/*": "resources/"
    }
  }
}
```

> **NOTE:** In Tauri’s [permission system](https://v2.tauri.app/reference/acl/capability/), absolute paths and paths containing parent components (`../`) can only be allowed via `"$RESOURCE/**"`. Relative paths like `"path/to/file.txt"` can be allowed explicitly via `"$RESOURCE/path/to/file.txt"`.

---
## Source path syntax

In the following explanations “target resource directory” is either the value after the colon in the object notation, or a reconstruction of the original file paths in the array notation.

- `"dir/file.txt"`: copies the `file.txt` file into the target resource directory.
- `"dir/"`: copies all files **and directories** _recursively_ into the target resource directory. Use this if you also want to preserve the file system structure of your files and directories.
- `"dir/*"`: copies all files in the `dir` directory _non-recursively_ (sub-directories will be ignored) into the target resource directory.
- `"dir/**`: throws an error because `**` only matches directories and therefore no files can be found.
- `"dir/**/*"`: copies all files in the `dir` directory _recursively_ (all files in `dir/` and all files in all sub-directories) into the target resource directory.
- `"dir/**/**`: throws an error because `**` only matches directories and therefore no files can be found.
---
## Bundling resources

In your tauri.conf.json, you add entries to the bundle > resources field.

For example:

```json
"bundle": {
  "resources": ["lang/*"]
}
```

This means that everything under lang/ (in your project root, next to tauri.conf.json) will be bundled into the app’s **resources folder** when you build it.

So if you have:

```
lang/
 └─ de.json
```

that file becomes available at runtime inside the app.

---
## Accessing files in Rust

Tauri provides a PathResolver via app.path() or handle.path().

You resolve the bundled resource like this:

```rust
use tauri::{Manager, path::BaseDirectory};
use std::fs::File;

tauri::Builder::default()
  .setup(|app| {
    let resource_path = app
      .path()
      .resolve("lang/de.json", BaseDirectory::Resource)?;

    let file = File::open(&resource_path).unwrap();
    let lang_de: serde_json::Value = serde_json::from_reader(file).unwrap();

    println!("{}", lang_de.get("hello").unwrap()); // "Guten Tag!"

    Ok(())
  })
  .invoke_handler(tauri::generate_handler![hello])
  .run(tauri::generate_context!())
  .expect("error while running tauri app");

#[tauri::command]
fn hello(handle: tauri::AppHandle) -> String {
    let resource_path = handle
      .path()
      .resolve("lang/de.json", BaseDirectory::Resource)
      .unwrap();

    let file = File::open(&resource_path).unwrap();
    let lang_de: serde_json::Value = serde_json::from_reader(file).unwrap();

    lang_de.get("hello").unwrap().as_str().unwrap().to_string()
}
```

- The setup function shows how to load and parse a file during startup.
- The #[tauri::command] example exposes this same logic to the frontend via invoke('hello').

---
## Accessing files in JavaScript

You have **two choices**:

### (a) Call your Rust command

```js
import { invoke } from '@tauri-apps/api/core';

const greeting = await invoke<string>('hello');
console.log(greeting); // "Guten Tag!"
```

### (b) Directly read resources with `plugin-fs`

You’ll need two things:

1. Add permissions to src-tauri/capabilities/default.json:    

```json
{
  "$schema": "../gen/schemas/desktop-schema.json",
  "identifier": "main-capability",
  "description": "Capability for the main window",
  "windows": ["main"],
  "permissions": [
    "core:default",
    "fs:allow-read-text-file",
    "fs:allow-resource-read-recursive"
  ]
}
```

1. - fs:allow-read-text-file → lets you read text files.
    - fs:allow-resource-read-recursive → lets you recursively access everything under $RESOURCE.
2. Then in JS:

```js
import { resolveResource } from '@tauri-apps/api/path';
import { readTextFile } from '@tauri-apps/plugin-fs';

const resourcePath = await resolveResource('lang/de.json');
const langDe = JSON.parse(await readTextFile(resourcePath));

console.log(langDe.hello); // "Guten Tag!"
```

Here resolveResource translates "lang/de.json" into the correct absolute path inside the app bundle, and readTextFile reads it.

---