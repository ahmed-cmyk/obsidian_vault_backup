# NestJS Exception Filters

Exception filters are a feature in NestJS that allow you to catch unhandled exceptions that occur within your application and apply custom logic to them. This provides a centralized way to handle errors and send consistent, user-friendly responses.

### Built-in Exception Layer

NestJS has a built-in exception layer that handles all unhandled exceptions across the application. When an exception is not caught by your code, this layer catches it and sends an appropriate HTTP response. For example, if a `NotFoundException` is thrown, it will automatically return a `404 Not Found` response.

### Catching Exceptions

To implement custom exception handling logic, you create an exception filter by implementing the `ExceptionFilter` interface and decorating the class with `@Catch()`.

```typescript
// src/common/filters/http-exception.filter.ts
import { ExceptionFilter, Catch, ArgumentsHost, HttpException, HttpStatus } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException) // Catches instances of HttpException
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
        message: exception.message || 'Internal server error',
      });
  }
}
```

*   The `@Catch()` decorator can take a single exception type or an array of exception types to listen for.
*   `ArgumentsHost` is a utility class that provides methods to retrieve the arguments being passed to the handler in a generic way (e.g., `switchToHttp()` for HTTP context).

### Applying Exception Filters

Exception filters can be applied at different levels:

*   **Method-scoped**: Applied to a specific route handler.
    ```typescript
    import { Controller, Get, UseFilters, HttpException, HttpStatus } from '@nestjs/common';
    import { HttpExceptionFilter } from './common/filters/http-exception.filter';

    @Controller('test')
    export class TestController {
      @Get('error')
      @UseFilters(new HttpExceptionFilter())
      throwError() {
        throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
      }
    }
    ```

*   **Controller-scoped**: Applied to all route handlers within a controller.
    ```typescript
    import { Controller, Get, UseFilters, HttpException, HttpStatus } from '@nestjs/common';
    import { HttpExceptionFilter } from './common/filters/http-exception.filter';

    @UseFilters(new HttpExceptionFilter())
    @Controller('test')
    export class TestController {
      @Get('error')
      throwError() {
        throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
      }
    }
    ```

*   **Global-scoped**: Applied to the entire application (recommended for most cases).
    ```typescript
    // src/main.ts
    import { NestFactory } from '@nestjs/core';
    import { AppModule } from './app.module';
    import { HttpExceptionFilter } from './common/filters/http-exception.filter';

    async function bootstrap() {
      const app = await NestFactory.create(AppModule);
      app.useGlobalFilters(new HttpExceptionFilter()); // Apply globally
      await app.listen(3000);
    }
    bootstrap();
    ```

### Catching All Exceptions

To catch all types of exceptions (not just `HttpException`), you can omit the argument in `@Catch()`.

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpStatus } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    const status = (exception instanceof HttpException)
      ? exception.getStatus()
      : HttpStatus.INTERNAL_SERVER_ERROR;

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
        message: (exception instanceof HttpException) ? exception.message : 'Internal server error',
      });
  }
}
```
