# **MYNESTJSPROJECT**

## 创建项目

```bash
$ npm i -g @nestjs/cli
$ nest new project-name
$ cd project-name
$ yarn run start:dev
```

nest -h 可以查看创建项目的一些快捷指令

## 添加swagger支持

### 命令

```bash
$ yarn add @nestjs/swagger swagger-ui-express   
```

### 修改main.ts

```bash
// main.ts 
import { NestFactory } from "@nestjs/core";
//swagger
import { SwaggerModule, DocumentBuilder } from "@nestjs/swagger";
import { AppModule } from "./app.module";

const listenPort = 3000;
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  console.log(`listen in location: ${listenPort}`);

  //swagger config
  const config = new DocumentBuilder()
    .setTitle("Cats example")
    .setDescription("The cats API description")
    .setVersion("1.0")
    .addTag("cats")
    .build();
  const document = SwaggerModule.createDocument(app, config);
  SwaggerModule.setup("swagger", app, document);

  await app.listen(3000);
}
bootstrap();
```

### 使用示例: app.controller.ts文件

```bash
//app.controller.ts

@Controller()
@ApiTags("APP总模块")
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  @ApiOperation({
    summary: "测试接口",
  })
  getHello(): string {
    return this.appService.getGoodBay();
  }
}
```

### 创建User模块

```bash
$ nest g interface user
$ nest g mo user
$ nest g service user
$ nest g co user
```

## swagger

```bash
@ApiQuery({ name: 'name', required: false })
@ApiQuery({ name: 'role', enum: UserRole })
@ApiParam({ name: 'id' })
@ApiBody({ description: '请输入massage' })
@Api
@ApiResponse({
	status: 200,
	description: '描述内容',
	type: Hellow
})
```

## token

### 安装jwt模块

```bash
yarn add @nestjs/jwt passport-jwt @types/passport-jwt
```

权限模块auto

```bash
nest g mo auto
//新建模块
nest g s auto
//新建服务
nest g co auto
```

## cors

CORS

### 跨源资源共享（`CORS`）是一种允许从另一个域请求资源的机制。在底层，`Nest` 使用了Express的[cors](https://github.com/expressjs/cors) 包，它提供了一系列选项，您可以根据自己的要求进行自定义。

```bash
# main.ts
$ app.enableCors();
```

## 链接数据库

`Nest` 与数据库无关，允许您轻松地与任何 `SQL` 或 `NoSQL` 数据库集成。根据您的偏好，您有许多可用的选项。一般来说，将 `Nest` 连接到数据库只需为数据库加载一个适当的 `Node.js` 驱动程序，就像使用 [Express](https://expressjs.com/en/guide/database-integration.html) 或 `Fastify` 一样。

您还可以直接使用任何通用的 `Node.js` 数据库集成库或 `ORM` ，例如 [MikroORM](https://mikro-orm.io/)也可以在[这里查看](https://docs.nestjs.com/recipes/mikroorm)、[Sequelize](https://sequelize.org/)（导航到[Sequelize 集成](https://docs.nestjs.com/techniques/database#sequelize-integration)部分）、[Knex.js](https://knexjs.org/)（[教程](https://dev.to/nestjs/build-a-nestjs-module-for-knex-js-or-other-resource-based-libraries-in-5-minutes-12an)）、[TypeORM](https://github.com/typeorm/typeorm)和[Prisma](https://www.github.com/prisma/prisma)（[教程](https://docs.nestjs.com/recipes/prisma)） ，以在更高的抽象级别上进行操作。

为了方便起见，`Nest` 还提供了与现成的 `TypeORM` 与 `@nestjs/typeorm` 的紧密集成，我们将在本章中对此进行介绍，而与 `@nestjs/mongoose` 的紧密集成将在[这一章](https://docs.nestjs.cn/8/techniques?id=mongo)中介绍。这些集成提供了附加的特定于 `nestjs` 的特性，比如模型/存储库注入、可测试性和异步配置，从而使访问您选择的数据库更加容易。

### TypeORM 集成

为了与 `SQL`和 `NoSQL` 数据库集成，`Nest` 提供了 `@nestjs/typeorm` 包。`Nest` 使用[TypeORM](https://github.com/typeorm/typeorm)是因为它是 `TypeScript` 中最成熟的对象关系映射器( `ORM` )。因为它是用 `TypeScript` 编写的，所以可以很好地与 `Nest` 框架集成。

为了开始使用它，我们首先安装所需的依赖项。在本章中，我们将演示如何使用流行的 [Mysql](https://www.mysql.com/) ， `TypeORM` 提供了对许多关系数据库的支持，比如 `PostgreSQL` 、`Oracle`、`Microsoft SQL Server`、`SQLite`，甚至像 `MongoDB`这样的 `NoSQL` 数据库。我们在本章中介绍的过程对于 `TypeORM` 支持的任何数据库都是相同的。您只需为所选数据库安装相关的客户端 `API` 库。

### 安装命令

```bash
yarn add @nestjs/typeorm typeorm mysql2
```

安装过程完成后，我们可以将 `TypeOrmModule` 导入`AppModule`

```
# app.module.ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'mysql',
      host: 'localhost',
      port: 3306,
      username: 'root',
      password: 'root',
      database: 'test',
      entities: [],
      synchronize: true,
    }),
  ],
})
export class AppModule {}
```

`forRoot()` 方法支持所有`TypeORM`包中`createConnection()`函数暴露出的配置属性。其他一些额外的配置参数描述如下：

| 参数                | 说明                                                     |
| ------------------- | -------------------------------------------------------- |
| retryAttempts       | 重试连接数据库的次数（默认：10）                         |
| retryDelay          | 两次重试连接的间隔(ms)（默认：3000）                     |
| autoLoadEntities    | 如果为`true`,将自动加载实体(默认：false)                 |
| keepConnectionAlive | 如果为`true`，在应用程序关闭后连接不会关闭（默认：false) |

