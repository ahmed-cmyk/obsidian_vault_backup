# NestJS Providers

Providers are fundamental to NestJS. Most basic NestJS classes can be treated as a provider. Services are a common type of provider. They are typically used for business logic, data access, or any functionality that can be injected into other parts of the application.

*   **`@Injectable()` decorator**: Marks a class as a provider that can be injected into other classes.
*   **Dependency Injection**: NestJS's dependency injection system manages the instantiation and lifecycle of providers.
