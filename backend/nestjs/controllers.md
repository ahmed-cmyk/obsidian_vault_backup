# NestJS Controllers

Controllers are responsible for handling incoming requests and returning responses to the client. They are defined using the `@Controller()` decorator.

*   **`@Controller()` decorator**: Defines a class as a controller. It can take an optional path prefix (e.g., `@Controller('users')`).
*   **HTTP Method Decorators**: Methods within a controller are associated with HTTP request methods using decorators like `@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`, `@Options()`, `@Head()`.
*   **Route Handlers**: Methods decorated with HTTP method decorators are called route handlers.
