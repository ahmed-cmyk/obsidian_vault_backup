# NestJS Data Transfer Objects (DTOs)

DTOs are plain TypeScript classes or interfaces that define the shape of data being sent over the network (e.g., from a client to the server in a request body, or from the server to the client in a response). They are crucial for validation, type safety, and clear communication of data structures.

NestJS often uses DTOs in conjunction with validation pipes (like `ValidationPipe`) to automatically validate incoming request payloads.

```typescript
// src/cats/dto/create-cat.dto.ts
import { IsString, IsInt, Min, MaxLength } from 'class-validator';

export class CreateCatDto {
  @IsString()
  @MaxLength(30)
  name: string;

  @IsInt()
  @Min(0)
  age: number;

  @IsString()
  breed: string;
}
```

To use validation decorators like `@IsString()`, you need to install `class-validator` and `class-transformer`:

```bash
npm install class-validator class-transformer
# OR
yarn add class-validator class-transformer
```
