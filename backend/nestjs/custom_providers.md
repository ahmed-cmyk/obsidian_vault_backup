# NestJS Custom Providers

NestJS's Dependency Injection (DI) system is powerful and flexible. While most providers are simple classes decorated with `@Injectable()`, you can create custom providers for more advanced scenarios, such as:

*   Injecting a value that is not a class (e.g., a string, number, or object).
*   Injecting a class that is instantiated conditionally.
*   Injecting a class that is not directly instantiated by NestJS (e.g., a third-party library instance).

Custom providers are defined using a provider object, which has a `provide` property and one of `useValue`, `useClass`, `useFactory`, or `useExisting`.

### `useValue`

Use `useValue` to inject a static value, a string, a number, or an object. This is useful for injecting configuration objects or external library instances.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

const config = {
  apiKey: 'your-api-key',
  databaseUrl: 'mongodb://localhost/nest',
};

@Module({
  imports: [],
  controllers: [AppController],
  providers: [
    AppService,
    {
      provide: 'CONFIG',
      useValue: config,
    },
  ],
})
export class AppModule {}
```

**Usage:**

```typescript
// src/app.service.ts
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class AppService {
  constructor(@Inject('CONFIG') private readonly config: any) {}

  getHello(): string {
    return `Hello from ${this.config.apiKey}`;
  }
}
```

### `useClass`

Use `useClass` to dynamically determine which class NestJS should instantiate and inject. This is useful for implementing different strategies or versions of a service.

```typescript
// src/common/services/abstract-logger.ts
export abstract class AbstractLogger {
  abstract log(message: string): void;
}

// src/common/services/console-logger.ts
import { Injectable } from '@nestjs/common';
import { AbstractLogger } from './abstract-logger';

@Injectable()
export class ConsoleLogger implements AbstractLogger {
  log(message: string): void {
    console.log(`ConsoleLogger: ${message}`);
  }
}

// src/common/services/file-logger.ts
import { Injectable } from '@nestjs/common';
import { AbstractLogger } from './abstract-logger';

@Injectable()
export class FileLogger implements AbstractLogger {
  log(message: string): void {
    // Logic to write to a file
    console.log(`FileLogger: ${message}`);
  }
}

// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { AbstractLogger } from './common/services/abstract-logger';
import { ConsoleLogger } from './common/services/console-logger';
import { FileLogger } from './common/services/file-logger';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [
    AppService,
    {
      provide: AbstractLogger,
      useClass: process.env.NODE_ENV === 'production' ? FileLogger : ConsoleLogger,
    },
  ],
})
export class AppModule {}
```

**Usage:**

```typescript
// src/app.service.ts
import { Injectable } from '@nestjs/common';
import { AbstractLogger } from './common/services/abstract-logger';

@Injectable()
export class AppService {
  constructor(private readonly logger: AbstractLogger) {}

  getHello(): string {
    this.logger.log('Hello from AppService!');
    return 'Hello World!';
  }
}
```

### `useFactory`

Use `useFactory` to create a provider dynamically based on some logic or other injected dependencies. The factory function can be `async`.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [
    AppService,
    {
      provide: 'DATABASE_CONNECTION',
      useFactory: async () => {
        // Simulate async connection to a database
        return new Promise((resolve) => {
          setTimeout(() => {
            console.log('Database connected!');
            resolve({ connection: 'MongoDB' });
          }, 1000);
        });
      },
    },
  ],
})
export class AppModule {}
```

**Usage:**

```typescript
// src/app.service.ts
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class AppService {
  constructor(@Inject('DATABASE_CONNECTION') private readonly connection: any) {}

  getHello(): string {
    return `Hello from database: ${this.connection.connection}`;
  }
}
```

### `useExisting`

Use `useExisting` to create an alias for an existing provider. This means that two different tokens can resolve to the same instance.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [
    AppService,
    {
      provide: 'APP_SERVICE_ALIAS',
      useExisting: AppService, // Alias for AppService
    },
  ],
})
export class AppModule {}
```

**Usage:**

```typescript
// src/app.controller.ts
import { Controller, Get, Inject } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(
    private readonly appService: AppService,
    @Inject('APP_SERVICE_ALIAS') private readonly appServiceAlias: AppService,
  ) {}

  @Get()
  getHello(): string {
    // Both appService and appServiceAlias refer to the same instance
    console.log(this.appService === this.appServiceAlias); // true
    return this.appService.getHello();
  }
}
```
