# NestJS Interceptors

Interceptors are a powerful feature in NestJS that allow you to bind extra logic before or after method execution. They are inspired by the Aspect-Oriented Programming (AOP) paradigm and can be used for a variety of cross-cutting concerns.

Common use cases for interceptors include:

*   **Logging**: Log user interaction, method calls, and errors.
*   **Transformation**: Transform the result returned from a route handler.
*   **Caching**: Implement a response cache.
*   **Error Handling**: Catch and transform exceptions.
*   **Authentication/Authorization**: Attach user information to the request.

### Creating an Interceptor

An interceptor is a class annotated with `@Injectable()` that implements the `NestInterceptor` interface.

```typescript
// src/common/interceptors/logging.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');

    const now = Date.now();
    return next
      .handle()
      .pipe(
        tap(() => console.log(`After... ${Date.now() - now}ms`)),
      );
  }
}
```

*   `intercept()` method: This method takes two arguments:
    *   `context`: An `ExecutionContext` instance, providing access to the current execution context.
    *   `next`: A `CallHandler` instance, which is an object that wraps the stream of execution. Calling `next.handle()` invokes the route handler.
*   `Observable`: Interceptors leverage RxJS `Observable`s for asynchronous operations. The `intercept` method must return an `Observable`.

### Applying Interceptors

Interceptors can be applied at different levels:

*   **Method-scoped**: Applied to a specific route handler.
    ```typescript
    import { Controller, Get, UseInterceptors } from '@nestjs/common';
    import { LoggingInterceptor } from './common/interceptors/logging.interceptor';

    @Controller('test')
    export class TestController {
      @Get('intercepted')
      @UseInterceptors(LoggingInterceptor)
      getIntercepted() {
        return 'This response is intercepted!';
      }
    }
    ```

*   **Controller-scoped**: Applied to all route handlers within a controller.
    ```typescript
    import { Controller, Get, UseInterceptors } from '@nestjs/common';
    import { LoggingInterceptor } from './common/interceptors/logging.interceptor';

    @UseInterceptors(LoggingInterceptor)
    @Controller('test')
    export class TestController {
      @Get('intercepted')
      getIntercepted() {
        return 'This response is intercepted!';
      }
    }
    ```

*   **Global-scoped**: Applied to the entire application.
    ```typescript
    // src/main.ts
    import { NestFactory } from '@nestjs/core';
    import { AppModule } from './app.module';
    import { LoggingInterceptor } from './common/interceptors/logging.interceptor';

    async function bootstrap() {
      const app = await NestFactory.create(AppModule);
      app.useGlobalInterceptors(new LoggingInterceptor()); // Apply globally
      await app.listen(3000);
    }
    bootstrap();
    ```

### Transformation Interceptor Example

An interceptor can transform the response before it is sent back to the client.

```typescript
// src/common/interceptors/transform.interceptor.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface Response<T> {
  data: T;
}

@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
  intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
    return next.handle().pipe(map(data => ({ data })));
  }
}
```

This interceptor wraps the response data in a `data` property. If applied globally, all successful responses would look like: `{ "data": { ...actual_response... } }`.
