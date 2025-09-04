# NestJS Guards

Guards are classes annotated with the `@Injectable()` decorator that implement the `CanActivate` interface. They are responsible for determining whether a given request will be handled by the route handler or not. This is often used for **authorization** (checking permissions).

Guards execute *after* all middleware and *before* any interceptors or pipes.

### Creating a Guard

A guard must implement the `CanActivate` interface, which requires a single `canActivate()` method.

```typescript
// src/common/guards/auth.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    // For demonstration, assume a simple check for a 'token' in headers
    return request.headers.authorization === 'Bearer my-secret-token';
  }
}
```

*   The `canActivate()` method returns a `boolean`, `Promise<boolean>`, or `Observable<boolean>`.
    *   If it returns `true`, the request will be processed.
    *   If it returns `false`, NestJS will deny the request and return a `403 Forbidden` response.

### Applying Guards

Guards can be applied at different levels:

*   **Method-scoped**: Applied to a specific route handler.
    ```typescript
    import { Controller, Get, UseGuards } from '@nestjs/common';
    import { AuthGuard } from './common/guards/auth.guard';

    @Controller('admin')
    export class AdminController {
      @Get('dashboard')
      @UseGuards(AuthGuard)
      getDashboard() {
        return 'Welcome to the admin dashboard!';
      }
    }
    ```

*   **Controller-scoped**: Applied to all route handlers within a controller.
    ```typescript
    import { Controller, Get, UseGuards } from '@nestjs/common';
    import { AuthGuard } from './common/guards/auth.guard';

    @UseGuards(AuthGuard)
    @Controller('admin')
    export class AdminController {
      @Get('dashboard')
      getDashboard() {
        return 'Welcome to the admin dashboard!';
      }

      @Get('settings')
      getSettings() {
        return 'Admin settings';
      }
    }
    ```

*   **Global-scoped**: Applied to the entire application.
    ```typescript
    // src/main.ts
    import { NestFactory } from '@nestjs/core';
    import { AppModule } from './app.module';
    import { AuthGuard } from './common/guards/auth.guard';

    async function bootstrap() {
      const app = await NestFactory.create(AppModule);
      app.useGlobalGuards(new AuthGuard()); // Apply globally
      await app.listen(3000);
    }
    bootstrap();
    ```

### Role-Based Access Control (RBAC) Example

Guards are often used for RBAC. You can combine guards with custom decorators to pass metadata.

```typescript
// src/common/decorators/roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const Roles = (...roles: string[]) => SetMetadata('roles', roles);
```

```typescript
// src/common/guards/roles.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const roles = this.reflector.get<string[]>('roles', context.getHandler());
    if (!roles) {
      return true; // No roles defined, allow access
    }

    const request = context.switchToHttp().getRequest();
    const user = request.user; // Assume user object is attached by an authentication middleware/guard

    // For demonstration, assume user has a 'roles' array
    return user && user.roles && roles.some(role => user.roles.includes(role));
  }
}
```

**Usage:**

```typescript
import { Controller, Get, UseGuards } from '@nestjs/common';
import { Roles } from './common/decorators/roles.decorator';
import { RolesGuard } from './common/guards/roles.guard';

@Controller('users')
@UseGuards(RolesGuard) // Apply the RolesGuard at the controller level
export class UsersController {
  @Get('admin-only')
  @Roles('admin') // Require 'admin' role for this route
  getAdminData() {
    return 'This data is only for admins.';
  }

  @Get('editor-or-admin')
  @Roles('editor', 'admin') // Require 'editor' or 'admin' role
  getEditorOrAdminData() {
    return 'This data is for editors or admins.';
  }
}
```
