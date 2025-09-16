# Project Structure

This note explains the typical directory and file structure of a React Native project, helping you understand where different parts of your application reside.

## Key Directories and Files

- `android/`: Contains the native Android project.
- `ios/`: Contains the native iOS project.
- `node_modules/`: Stores all installed Node.js modules.
- `src/`: (Commonly created by developers) Contains your application's source code.
- `App.js`: The main component of your React Native application.
- `package.json`: Manages project dependencies and scripts.
- `index.js`: The entry point for your React Native application.

## Understanding the Structure

- **Native Folders (`android/`, `ios/`):** These folders hold the actual native projects for each platform. You'll interact with these when dealing with native modules, build configurations, or platform-specific code.
- **JavaScript Files:** Your React Native code primarily lives in `.js` or `.jsx` files. `App.js` is usually the root component.
- **`package.json`:** Essential for managing your project's dependencies and defining scripts for tasks like starting the development server or running tests.