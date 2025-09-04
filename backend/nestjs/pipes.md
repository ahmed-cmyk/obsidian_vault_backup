# NestJS Pipes

Pipes are classes annotated with the `@Injectable()` decorator that implement the `PipeTransform` interface. They are used for two main purposes:

1.  **Transformation**: Transform input data to a desired format (e.g., string to number).
2.  **Validation**: Evaluate input data and throw an exception if the data is invalid.

NestJS provides several built-in pipes, such as `ValidationPipe`, `ParseIntPipe`, `ParseUUIDPipe`, etc.

#### Using `ValidationPipe`

The `ValidationPipe` is commonly used with DTOs to automatically validate incoming request bodies based on the validation decorators (`class-validator`) applied to the DTO properties.

You can apply pipes at different levels:

*   **Method-level**: Applied to a specific route handler.
    ```typescript
    import { Controller, Post, Body, UsePipes, ValidationPipe } from '@nestjs/common';
    import { CreateCatDto } from './dto/create-cat.dto';

    @Controller('cats')
    export class CatsController {
      @Post()
      @UsePipes(new ValidationPipe())
      create(@Body() createCatDto: CreateCatDto) {
        // createCatDto is now validated
        return 'This action adds a new cat';
      }
    }
    ```

*   **Parameter-level**: Applied to a specific parameter.
    ```typescript
    import { Controller, Get, Param, ParseIntPipe } from '@nestjs/common';

    @Controller('users')
    export class UsersController {
      @Get(':id')
      findOne(@Param('id', ParseIntPipe) id: number): string {
        // id is guaranteed to be a number
        return `This action returns a user with ID: ${id}`;
      }
    }
    ```

*   **Global-level**: Applied to the entire application (recommended for `ValidationPipe`).
    ```typescript
    // src/main.ts
    import { NestFactory } from '@nestjs/core';
    import { AppModule } from './app.module';
    import { ValidationPipe } from '@nestjs/common';

    async function bootstrap() {
      const app = await NestFactory.create(AppModule);
      app.useGlobalPipes(new ValidationPipe()); // Apply globally
      await app.listen(3000);
    }
    bootstrap();
    ```

#### Custom Pipes

You can create custom pipes by implementing the `PipeTransform` interface.

```typescript
// src/common/pipes/parse-int.pipe.ts
import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';

@Injectable()
export class ParseIntPipe implements PipeTransform<string, number> {
  transform(value: string, metadata: ArgumentMetadata): number {
    const val = parseInt(value, 10);
    if (isNaN(val)) {
      throw new BadRequestException('Validation failed: ' + metadata.data + ' must be an integer');
    }
    return val;
  }
}
```
