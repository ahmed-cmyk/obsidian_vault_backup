
Since Tauri is a toolkit for building applications there can be many files to configure project settings. Some common files that you may run across are `tauri.conf.json`, `package.json` and `Cargo.toml`.

---
## `tauri.conf.json`

The Tauri configuration file defines your web app’s entry point, application metadata, bundle settings, plugin options, and runtime behavior (such as windows, menus, and tray icons).  

It is read by both the Tauri runtime and the CLI. In it, you can specify build commands (run before `tauri build` or `tauri dev`), set your app’s name and version, manage runtime options, and configure plugins.

You can modify the app's dev URL and the script that starts it by using the following configuration:

```json
// tauri.conf.json
{
  "build": {
    "devUrl": "http://localhost:3000",
    "beforeDevCommand": "npm run dev"
  }
}
```

If you are not using a UI framework or module bundler you can point Tauri to your frontend source code and the Tauri CLI will start a development server for you:

```json
// tauri.conf.json
{
  "build": {
    "frontendDist": "./src"
  }
}
```

> **NOTE:** In this example the `src` folder must include a `index.html` file along any other assets loaded by your frontend.

### Supported Formats

The default Tauri config format is JSON. The JSON5 or TOML format can be enabled by adding the `config-json5` or `config-toml` feature flag (respectively) to the `tauri` and `tauri-build` dependencies in `Cargo.toml`.

```toml
# Cargo.toml
[build-dependencies]
tauri-build = { version = "2.0.0", features = [ "config-json5" ] }

[dependencies]
tauri = { version = "2.0.0", features = [  "config-json5" ] }
```

The structure and values are the same across all formats, however, the formatting should be consistent with the respective file’s format:

```json
// tauri.conf.json
{
  build: {
    devUrl: 'http://localhost:3000',
    // start the dev server
    beforeDevCommand: 'npm run dev',
  },
  bundle: {
    active: true,
    icon: ['icons/app.png'],
  },
  app: {
    windows: [
      {
        title: 'MyApp',
      },
    ],
  },
  plugins: {
    updater: {
      pubkey: 'updater pub key',
      endpoints: ['https://my.app.updater/{{target}}/{{current_version}}'],
    },
  },
}
```

```toml
# Tauri.toml
[build]
dev-url = "http://localhost:3000"
# start the dev server
before-dev-command = "npm run dev"

[bundle]
active = true
icon = ["icons/app.png"]

[[app.windows]]
title = "MyApp"

[plugins.updater]
pubkey = "updater pub key"
endpoints = ["https://my.app.updater/{{target}}/{{current_version}}"]
```

> **NOTE:** JSON5 and TOML supports comments, and TOML can use kebab-case for config names which are more idiomatic. Field names are case-sensitive in all 3 formats.

### Platform-specific Configuration

In addition to the default configuration file, Tauri can read a platform-specific configuration from:

- `tauri.linux.conf.json` or `Tauri.linux.toml` for Linux
- `tauri.windows.conf.json` or `Tauri.windows.toml` for Windows
- `tauri.macos.conf.json` or `Tauri.macos.toml` for macOS
- `tauri.android.conf.json` or `Tauri.android.toml` for Android
- `tauri.ios.conf.json` or `Tauri.ios.toml` for iOS

The platform-specific configuration file gets merged with the main configuration object following the [JSON Merge Patch (RFC 7396)](https://datatracker.ietf.org/doc/html/rfc7396) specification.

```json
// tauri.conf.json
{
  "productName": "MyApp",
  "bundle": {
    "resources": ["./resources"]
  },
  "plugins": {
    "deep-link": {}
  }
}
```

And the given `tauri.linux.conf.json`:

```json
// tauri.linux.conf.json
{
  "productName": "my-app",
  "bundle": {
    "resources": ["./linux-assets"]
  },
  "plugins": {
    "cli": {
      "description": "My app",
      "subcommands": {
        "update": {}
      }
    },
    "deep-link": {}
  }
}
```

The resolved configuration for Linux would be the following object:

```json
{
  "productName": "my-app",
  "bundle": {
    "resources": ["./linux-assets"]
  },
  "plugins": {
    "cli": {
      "description": "My app",
      "subcommands": {
        "update": {}
      }
    },
    "deep-link": {}
  }
}
```

---
## Cargo.toml

`Cargo.toml` is Cargo’s manifest file. It defines your app’s Rust dependencies (crates), project metadata, and other Rust-related settings.  

If you aren’t doing backend Rust development, you may not modify it much — but it’s important to understand its role.

### Example (minimal Tauri project)

```toml
[package]
name = "app"
version = "0.1.0"
description = "A Tauri App"
authors = ["you"]
edition = "2021"
rust-version = "1.57"

[build-dependencies]
tauri-build = { version = "2.0.0" }

[dependencies]
serde_json = "1.0"
serde = { version = "1.0", features = ["derive"] }
tauri = { version = "2.0.0", features = [] }
````

### Key Points

- **Tauri dependencies**: tauri-build and tauri should usually match the CLI’s latest minor release. Check versions if you hit runtime errors.
- **Semantic Versioning**: Cargo uses SemVer. Running cargo update pulls the latest compatible versions.
    - Example: 2.0.0 will resolve to the latest 2.0.x.
    - Use exact versions with =, e.g. version = "=2.0.0".
- **Features**: The features=[] section is auto-managed by tauri dev / tauri build based on your config. See docs for feature flags.
- **Cargo.lock**: Generated at build time. Ensures consistent dependencies across machines (similar to package-lock.json in Node.js). Commit it to version control.

📚 See [Cargo’s official docs](https://doc.rust-lang.org/cargo/reference/manifest.html) for full details.

---
## package.json

`package.json` is Node.js’s package manifest. In a Tauri app, it manages **frontend dependencies** (React, Vue, etc.) and defines **scripts** for development and build tasks.

### Example (minimal Tauri project)

```json
{
  "scripts": {
    "dev": "command to start your app development mode",
    "build": "command to build your app frontend",
    "tauri": "tauri"
  },
  "dependencies": {
    "@tauri-apps/api": "^2.0.0",
    "@tauri-apps/cli": "^2.0.0"
  }
}
````

### Key Points

- **Scripts**
    - `dev` → runs the frontend dev server (`npm run dev` / `yarn dev`).
    - `build` → builds production frontend assets.
    - `tauri` → required only if you use **npm** (not yarn/pnpm).
    
- **Integration with Tauri**
    - Scripts are typically hooked into Tauri via `tauri.conf.json`:

```json
{
  "build": {
    "beforeDevCommand": "yarn dev",
    "beforeBuildCommand": "yarn build"
  }
}
```

- **Dependencies**    
    - `@tauri-apps/api` → lets the frontend call Tauri APIs.
    - `@tauri-apps/cli` → CLI wrapper for Tauri (installed here if not global).
    
- **Lockfiles**
    - `yarn.lock`, `pnpm-lock.yaml`, or `package-lock.json` ensure consistent installs (similar to `Cargo.lock` in Rust).

📚 See [npm’s package.json docs](https://docs.npmjs.com/cli/v9/configuring-npm/package-json) for full details.

---
