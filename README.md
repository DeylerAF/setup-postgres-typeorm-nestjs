# Setup PostgreSQL database with TypeORM and NestJS

> [!TIP]
>To connect a NestJS project with a PostgreSQL database, you can use the @nestjs/typeorm module, which is a NestJS wrapper for TypeORM, a popular Object-Relational Mapping (ORM) tool for TypeScript and JavaScript.

## Here are the steps to set up the connection:
1. **Install the necessary dependencies:**

```bash
npm install @nestjs/typeorm typeorm pg
```

2. **Import the TypeOrmModule into your application module (app.module.ts or another relevant module) and use the forRoot method to configure it:**
> [!NOTE]
> Replace 'localhost', 5432, 'test', 'test', and 'test' with your actual PostgreSQL host, port, username, password, and database name. The entities option should point to your entities, and synchronize: true will automatically create the database tables for your entities on every application launch.

```javascript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: 'localhost',
      port: 5432,
      username: 'test',
      password: 'test',
      database: 'test',
      entities: [__dirname + '/**/*.entity{.ts,.js}'],
      synchronize: true,
    }),
    // other modules...
  ],
  // controllers, providers...
})
export class AppModule {}
```

3. **Now you can use the TypeOrmModule.forFeature method in other modules to get repository instances for your entities:**

```javascript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { MyEntity } from './my.entity';
import { MyService } from './my.service';
import { MyController } from './my.controller';

@Module({
  imports: [TypeOrmModule.forFeature([MyEntity])],
  providers: [MyService],
  controllers: [MyController],
})
export class MyModule {}
```

4. **In your services, you can inject the repository instances and use them to interact with the database:**
> [!NOTE]
> Remember to replace MyEntity, MyService, and MyController with your actual entity, service, and controller classes.

```javascript
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { MyEntity } from './my.entity';

@Injectable()
export class MyService {
  constructor(
    @InjectRepository(MyEntity)
    private myEntityRepository: Repository<MyEntity>,
  ) {}

  findAll(): Promise<MyEntity[]> {
    return this.myEntityRepository.find();
  }

  // other methods...
}
```
