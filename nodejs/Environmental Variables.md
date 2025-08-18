# Environmental Variables

## `process.env`

The `process` core module of NodeJS provides the `env` property which hosts all the environment variables that were set at the moment the process was started.

> **NOTE**: process does not need to be imported, it is a global object in Node.js.

The following is an example of passing env variables as arguments:

```js
USER_ID=239482 USER_KEY=foobar node app.js
```

Accessing the variables:

```js
console.log(process.env.USER_ID); // "239482"
console.log(process.env.USER_KEY); // "foobar"
```

> **NOTE**: NodeJS 20 introduced experimental support for `.env` files.

Now, you can use the `--env-file` flag to specify an environment file when running your Node.js application.

```bash
node --env-file=.env app.js
```

You can pass multiple --env-file arguments:

```bash
node --env-file=.env --env-file=.development.env app.js
```

> **NOTE**: if the same variable is defined in the environment and in the file, the value from the environment takes precedence.

If you want to optionally read from a .env file, it's possible to avoid throwing an error if the file is missing using the --env-file-if-exists flag.

```bash
node --env-file-if-exists=.env app.js
```

---
Here’s a **companion section for `dotenv`**, and also a note on **preferred approach**:

---
## `dotenv`

The [`dotenv`](https://www.npmjs.com/package/dotenv) package is a popular, widely used utility for loading environment variables from a `.env` file into `process.env`in Node.js applications.
It is commonly used in projects that need to work on older Node.js versions (pre-v20) or want explicit control over `.env`parsing and behavior.

> **NOTE:** Unlike `process.env`, you need to install and require `dotenv` explicitly.

### Installing `dotenv`

```bash
npm install dotenv
```

### Using `dotenv`

At the very start of your application (ideally the first line in your main file), call `config()`:

```js
require('dotenv').config();

console.log(process.env.USER_ID);
console.log(process.env.USER_KEY);
```

By default, it loads variables from a file named `.env` in the project root.

### Customizing the file

You can specify another file:

```js
require('dotenv').config({ path: './.development.env' });
```

---
## Why use `dotenv`?

✅ Works with all Node versions.
✅ Fails gracefully if `.env` file is missing.
✅ Supports comments and empty lines in `.env` files.
✅ Very mature and stable — the de facto standard for Node.js applications.
✅ Portable — works even when deploying to environments where `--env-file` isn’t supported.

---
## Preferred Approach: `.env` with dotenv or `--env-file`?

* If you’re using **Node.js ≥ v20**, the recommended (and future-proof) way is to use the native `--env-file` flag — no extra dependencies needed.

* If you want to support older versions of Node, or need additional flexibility or control (e.g., ignoring missing files, using multiple `.env` files conditionally, parsing `.env` in tests), then use `dotenv`.

### Rule of thumb:

| Use case | Preferred approach |
|---|---|
| Node.js v20+ only? | `--env-file` | 
| Need compatibility with older Node? | `dotenv` | 
| Need advanced behavior (ignore missing file, multiple files, programmatic control)? | `dotenv` | 
| Running in environments where CLI flags are not configurable? | `dotenv` | 
---

### Example `.env` file

```bash
USER_ID=239482
USER_KEY=foobar
API_URL=https://example.com/api
DEBUG=true
```

> **NOTE**: Both `dotenv` and `--env-file` will populate these into `process.env` at runtime.

---

#nodejs