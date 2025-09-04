# NestJS Project Structure

A newly created NestJS project typically has the following structure:

```
project-name/
├── .git/
├── .gitignore
├── .prettierrc
├── nest-cli.json
├── package.json
├── README.md
├── tsconfig.build.json
├── tsconfig.json
└── src/
    ├── main.ts
    ├── app.controller.spec.ts
    ├── app.controller.ts
    ├── app.module.ts
    ├── app.service.ts
    └── assets/ (optional, for static assets)
```

### Key Files and Directories:

*   `src/`: Contains the main application source code.
    *   `main.ts`: The entry file of the application. It bootstraps the NestJS application.
    *   `app.module.ts`: The root module of the application. Modules are used to organize code.
    *   `app.controller.ts`: A basic controller that handles incoming requests and returns responses.
    *   `app.service.ts`: A basic service that provides data and logic.
    *   `*.spec.ts`: Test files for controllers and services.
*   `nest-cli.json`: Configuration file for the NestJS CLI.
*   `package.json`: Standard Node.js package manifest, listing dependencies and scripts.
*   `tsconfig.json`: TypeScript configuration file.
*   `.prettierrc`: Prettier configuration for code formatting.
