# NestJS Custom Decorators

NestJS allows you to create custom decorators to enhance your application's functionality and improve code readability. Custom decorators are built on top of NestJS's `createParamDecorator` function.

They are particularly useful for extracting specific data from the request object or for applying common logic across multiple route handlers.

```typescript
// src/common/decorators/user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const User = createParamDecorator(
  (data: string, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user; // Assuming user information is attached to the request object (e.g., by an authentication guard)

    return data ? user?.[data] : user;
  },
);
```

**Usage:**

```typescript
import { Controller, Get } from '@nestjs/common';
import { User } from '../common/decorators/user.decorator';

interface RequestUser {
  firstName: string;
  lastName: string;
  roles: string[];
}

@Controller('profile')
export class ProfileController {
  @Get()
  getProfile(@User() user: RequestUser) {
    console.log(user);
    return `Hello ${user.firstName}`;
  }

  @Get('name')
  getName(@User('firstName') firstName: string) {
    return `Your first name is ${firstName}`;
  }
}
```

*   The `createParamDecorator` function takes a factory function as an argument. This factory function receives `data` (the argument passed to the decorator, e.g., `'firstName'` in `@User('firstName')`) and `ctx` (the `ExecutionContext`).
*   The `ExecutionContext` provides utilities to access the underlying request, response, and other execution context details.
