# NestJS Modules

Modules are classes annotated with a `@Module()` decorator. They provide metadata that NestJS uses to organize the application structure. Every NestJS application has at least one root module, conventionally named `AppModule`.

*   **`@Module()` decorator**: Takes an object with the following properties:
    *   `imports`: A list of imported modules that export the providers that are needed in this module.
    *   `controllers`: A list of controllers defined in this module.
    *   `providers`: A list of providers (services, factories, etc.) that will be instantiated by the NestJS injector and that can be shared across the application.
    *   `exports`: A subset of the `providers` that should be available to other modules that import this module.
