
In your project repository, you **SHOULD** commit the `src-tauri/Cargo.lock` along with the `src-tauri/Cargo.toml` to git because Cargo uses the lockfile to provide deterministic builds. As a result, it is recommended that all applications check in their `Cargo.lock`. You **SHOULD NOT** commit the `src-tauri/target` folder or any of its contents.

---
