# NestJS Installation

To get started with NestJS, you need to have Node.js (LTS version recommended) and npm (or yarn) installed on your system.

### Prerequisites

*   Node.js (version 10.13.0 or higher)
*   npm (comes with Node.js) or Yarn

### Using the NestJS CLI

The NestJS CLI (Command Line Interface) is the recommended way to scaffold NestJS projects. It provides tools for generating boilerplate code, running tests, and building applications.

1.  **Install the CLI globally**:
    ```bash
    npm i -g @nestjs/cli
    # OR
    yarn global add @nestjs/cli
    ```

2.  **Create a new NestJS project**:
    ```bash
    nest new project-name
    ```
    This command will create a new directory `project-name` with a basic NestJS project structure. It will also install all necessary dependencies.

    You will be prompted to choose a package manager (npm, yarn, or pnpm).

3.  **Navigate into your project directory**:
    ```bash
    cd project-name
    ```

4.  **Start the application**:
    ```bash
    npm run start
    # OR
    yarn start
    ```
    This will start the development server, typically on `http://localhost:3000`.
