# NestJS TypeORM Integration

TypeORM is a powerful ORM that can run in NodeJS, Browser, Cordova, PhoneGap, Ionic, React Native, NativeScript, Expo, and Electron platforms and can be used with TypeScript and JavaScript (ES5, ES6, ES8). It supports MySQL, PostgreSQL, MariaDB, SQLite, MS SQL Server, Oracle, SAP Hana, and CockroachDB.

#### Installation

First, install the necessary packages:

```bash
npm install @nestjs/typeorm typeorm pg # or mysql, sqlite3, etc.
# OR
yarn add @nestjs/typeorm typeorm pg
```

#### Configuration

TypeORM is typically configured in the root `AppModule` using `TypeOrmModule.forRoot()`.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { User } from './users/user.entity'; // Import your entity

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: 'localhost',
      port: 5432,
      username: 'your_username',
      password: 'your_password',
      database: 'your_database',
      entities: [User], // List all your entities here
      synchronize: true, // Auto-create database schema (use only in development!)
    }),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

*   `entities`: An array of entity classes that TypeORM should manage.
*   `synchronize`: Set to `true` in development to automatically create database tables based on your entities. **Never use in production!** For production, use migrations.

#### Defining Entities

Entities are classes that map to database tables. They are decorated with `@Entity()` and their properties with `@Column()`, `@PrimaryGeneratedColumn()`, etc.

```typescript
// src/users/user.entity.ts
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({ unique: true })
  username: string;

  @Column()
  email: string;

  @Column({ default: true })
  isActive: boolean;
}
```

#### Using Repositories

NestJS integrates with TypeORM repositories, allowing you to inject them into your services to perform database operations.

```typescript
// src/users/users.module.ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';
import { User } from './user.entity';

@Module({
  imports: [TypeOrmModule.forFeature([User])], // Register entities for this feature module
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService], // Export if other modules need to use UsersService
})
export class UsersModule {}
```

```typescript
// src/users/users.service.ts
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './user.entity';

@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User) // Inject the User repository
    private usersRepository: Repository<User>,
  ) {}

  findAll(): Promise<User[]> {
    return this.usersRepository.find();
  }

  findOne(id: number): Promise<User> {
    return this.usersRepository.findOneBy({ id });
  }

  async create(user: Partial<User>): Promise<User> {
    const newUser = this.usersRepository.create(user);
    return this.usersRepository.save(newUser);
  }

  async remove(id: number): Promise<void> {
    await this.usersRepository.delete(id);
  }
}
```
