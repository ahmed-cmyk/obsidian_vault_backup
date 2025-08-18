# File Structure

```
src/
	components/
		App/
			index.js
		Header/
			index.js
		Widget/
			index.js
	helpers/
	hooks/
	constants.js
	utils.js
```

The `index.js` file contains forwards imports for all the other files inside the same directory. It cleans up the syntax during imports since the bundler looks for an `index.js` file when importing a directory and just uses that.

**Example:**

```jsx
import FileViewer from '../FileViewer/FileViewer'; // Without
import FileViewer from '../FileViewer'; // With index.js forwarding
```

**In general, there are two broad ways to organize things:**

- By function (components, hooks, helpers…)
- By feature (search, users, admin…)

**Here's an example of how to structure code by feature:**

```
src/
├── base/
│   └── components/
│       ├── Button.js
│       ├── Dropdown.js
│       ├── Heading.js
│       └── Input.js
├── search/
│   ├── components/
│   │   ├── SearchInput.js
│   │   └── SearchResults.js
│   └── search.helpers.js
└── users/
    ├── components/
    │   ├── AuthPage.js
    │   ├── ForgotPasswordForm.js
    │   └── LoginForm.js
    └── use-user.hook.js
```

---

## Hooks

Hooks that are specific to a component are placed alongside the component, and every other hook is placed in its own folder.

> **Note**: Add `.hook` at the end of each file name. Maybe use kebab-case when naming `.hook` files.

---

## Helpers

Helpers that are specific to a component are placed alongside the component, and every other helper is placed in its own folder.

> **Note**: Add `.helpers` at the end of each file name.

---

## Constants

> The `constants.js` file holds app-wide constants. Most of them are style-related (eg. colors, font sizes, breakpoints), but I also store public keys and other “app data” here.

**Source**: [File structure | Josh Comeau](https://www.joshwcomeau.com/react/file-structure/)

---
#reactjs