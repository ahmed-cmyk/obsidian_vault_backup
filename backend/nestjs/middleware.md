# NestJS Middleware

Middleware are functions that have access to the request and response objects, and the `next` middleware function in the application’s request-response cycle. They can perform the following tasks:

*   Execute any code.
*   Make changes to the request and the response objects.
*   End the request-response cycle.
*   Call the next middleware function in the stack.

NestJS middleware are equivalent to Express middleware.

### Functional Middleware

Simple middleware can be implemented as a function.

```typescript
// src/common/middleware/logger.middleware.ts
import { Request, Response, NextFunction } from 'express';

export function logger(req: Request, res: Response, next: NextFunction) {
  console.log(`Request...`);
  next();
};
```

### Class-based Middleware

For more complex middleware, you can use a class that implements the `NestMiddleware` interface.

```typescript
// src/common/middleware/logger.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log('Request...');
    next();
  }
}
```

### Applying Middleware

Middleware is registered within the `configure()` method of a module. This method is part of the `NestModule` interface.

```typescript
// src/app.module.ts
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { LoggerMiddleware } from './common/middleware/logger.middleware';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('*'); // Apply to all routes
  }
}
```

*   `apply()`: Takes the middleware class or function.
*   `forRoutes()`: Specifies the routes to which the middleware should be applied. It can take strings, `Controller` classes, or route objects.
    *   `'*'`: Applies to all routes.
    *   `'cats'`: Applies to routes starting with `/cats`.
    *   `CatsController`: Applies to all routes defined in `CatsController`.
    *   `{ path: 'cats', method: RequestMethod.GET }`: Applies to specific path and HTTP method.

### Excluding Routes

You can exclude specific routes from middleware.

```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude(
    { path: 'cats', method: RequestMethod.GET },
    'cats/(.*)',
  )
  .forRoutes(CatsController);
```

### Multiple Middleware

You can apply multiple middleware to the same routes.

```typescript
consumer
  .apply(LoggerMiddleware, AnotherMiddleware)
  .forRoutes('cats');
```

### Global Middleware

While middleware is typically configured within modules, you can also set up global middleware directly in `main.ts`.

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import * as express from 'express';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.use(express.json()); // Example of using an Express middleware globally
  app.use(express.urlencoded({ extended: true }));
  await app.listen(3000);
}
bootstrap();
```

Note that global middleware registered with `app.use()` are executed before any module-bound middleware.
