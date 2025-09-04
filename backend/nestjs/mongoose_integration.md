# NestJS Mongoose Integration

Mongoose is an ODM for MongoDB and Node.js. It provides a straightforward, schema-based solution to model your application data.

#### Installation

First, install the necessary packages:

```bash
npm install @nestjs/mongoose mongoose
# OR
yarn add @nestjs/mongoose mongoose
```

#### Configuration

Mongoose is typically configured in the root `AppModule` using `MongooseModule.forRoot()`.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [
    MongooseModule.forRoot('mongodb://localhost/nest'), // Your MongoDB connection string
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

#### Defining Schemas and Models

In Mongoose, you define schemas that map to MongoDB collections. Models are then created from these schemas to interact with the database.

```typescript
// src/cats/schemas/cat.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

export type CatDocument = Cat & Document;

@Schema()
export class Cat {
  @Prop()
  name: string;

  @Prop()
  age: number;

  @Prop()
  breed: string;
}

export const CatSchema = SchemaFactory.createForClass(Cat);
```

#### Using Models

NestJS integrates with Mongoose models, allowing you to inject them into your services to perform database operations.

```typescript
// src/cats/cats.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';
import { Cat, CatSchema } from './schemas/cat.schema';

@Module({
  imports: [MongooseModule.forFeature([{ name: Cat.name, schema: CatSchema }])], // Register models for this feature module
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService], // Export if other modules need to use CatsService
})
export class CatsModule {}
```

```typescript
// src/cats/cats.service.ts
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Cat, CatDocument } from './schemas/cat.schema';
import { CreateCatDto } from './dto/create-cat.dto';

@Injectable()
export class CatsService {
  constructor(@InjectModel(Cat.name) private catModel: Model<CatDocument>) {}

  async create(createCatDto: CreateCatDto): Promise<Cat> {
    const createdCat = new this.catModel(createCatDto);
    return createdCat.save();
  }

  async findAll(): Promise<Cat[]> {
    return this.catModel.find().exec();
  }

  async findOne(id: string): Promise<Cat> {
    return this.catModel.findById(id).exec();
  }

  async delete(id: string): Promise<any> {
    return this.catModel.deleteOne({ _id: id }).exec();
  }
}
```
