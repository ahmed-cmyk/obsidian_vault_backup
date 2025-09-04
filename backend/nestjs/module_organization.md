# NestJS Module Organization

As NestJS applications grow, organizing modules effectively becomes crucial for maintainability, scalability, and team collaboration. NestJS encourages a modular architecture, where each module encapsulates a specific feature or domain.

### Feature Modules

Feature modules are used to organize code related to a specific feature of your application (e.g., `UsersModule`, `ProductsModule`, `AuthModule`). Each feature module typically contains its own controllers, providers, and potentially other modules.

**Example Structure:**

```
src/
├── app.module.ts
├── main.ts
├── users/
│   ├── users.module.ts
│   ├── users.controller.ts
│   ├── users.service.ts
│   └── dto/
│       └── create-user.dto.ts
├── products/
│   ├── products.module.ts
│   ├── products.controller.ts
│   ├── products.service.ts
│   └── interfaces/
│       └── product.interface.ts
└── common/
    ├── filters/
    ├── guards/
    ├── interceptors/
    └── pipes/
```

**`users.module.ts` example:**

```typescript
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService], // If UsersService needs to be used by other modules
})
export class UsersModule {}
```

**Importing Feature Modules:**

Feature modules are imported into the root `AppModule` or other feature modules that depend on them.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { UsersModule } from './users/users.module';
import { ProductsModule } from './products/products.module';

@Module({
  imports: [UsersModule, ProductsModule],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

### Shared Modules

Shared modules (or common modules) are used to encapsulate and export providers, components, or other modules that are used across multiple feature modules. This prevents code duplication and promotes reusability.

**Example:** A `DatabaseModule` that provides a database connection, or a `ConfigModule` that provides configuration services.

```typescript
// src/database/database.module.ts
import { Module, Global } from '@nestjs/common';
import { createConnection } from 'typeorm'; // Example with TypeORM

@Global() // Makes providers available everywhere without explicit import
@Module({
  providers: [
    {
      provide: 'DATABASE_CONNECTION',
      useValue: createConnection(), // Actual database connection logic
    },
  ],
  exports: ['DATABASE_CONNECTION'],
})
export class DatabaseModule {}
```

*   The `@Global()` decorator makes the module global, meaning its exported providers are available everywhere without needing to import the module in every `imports` array. Use it sparingly, primarily for core utilities like database connections or configuration.

### Dynamic Modules

Dynamic modules allow you to create modules that can be configured at runtime. This is particularly useful for creating reusable modules that require specific configuration (e.g., a `ConfigModule` that loads different configurations based on the environment).

Dynamic modules typically expose static methods like `forRoot()` or `forFeature()` that return a `DynamicModule` object.

**Example: `ConfigModule` as a Dynamic Module**

```typescript
// src/config/config.module.ts
import { Module, DynamicModule } from '@nestjs/common';
import { ConfigService } from './config.service';

interface ConfigModuleOptions {
  folder: string;
}

@Module({})
export class ConfigModule {
  static forRoot(options: ConfigModuleOptions): DynamicModule {
    return {
      module: ConfigModule,
      providers: [
        {
          provide: 'CONFIG_OPTIONS',
          useValue: options,
        },
        ConfigService,
      ],
      exports: [ConfigService],
    };
  }
}
```

```typescript
// src/config/config.service.ts
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class ConfigService {
  private readonly envConfig: { [key: string]: any };

  constructor(@Inject('CONFIG_OPTIONS') private options: any) {
    // Load config from options.folder or other sources
    this.envConfig = { /* ... loaded config ... */ };
  }

  get(key: string): any {
    return this.envConfig[key];
  }
}
```

**Usage:**

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from './config/config.module';

@Module({
  imports: [
    ConfigModule.forRoot({ folder: './config' }),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

*   `forRoot()`: Conventionally used for modules that are configured once at the root level of the application.
*   `forFeature()`: Conventionally used for modules that provide features specific to a particular domain or feature module (e.g., `TypeOrmModule.forFeature([User])`).

### Module Re-exporting

You can re-export modules from within another module. This is useful when you want to expose a set of related modules through a single entry point.

```typescript
// src/common/common.module.ts
import { Module } from '@nestjs/common';
import { AuthModule } from '../auth/auth.module';
import { UsersModule } from '../users/users.module';

@Module({
  imports: [AuthModule, UsersModule],
  exports: [AuthModule, UsersModule], // Re-exporting
})
export class CommonModule {}
```

Now, other modules can just import `CommonModule` to get `AuthModule` and `UsersModule`.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { CommonModule } from './common/common.module';

@Module({
  imports: [CommonModule],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```
