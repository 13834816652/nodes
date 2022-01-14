# **Express+TypeScript**

### 通过 npx命令来运行 Express 应用程序生成器

```bash
npx express-generator
pnpm install express-generator
/npx包含在 Node.js 8.2.0 及更高版本中
```

### 安装依赖

```bash
yarn install
```

### 安装TS

```bash
yarn global add typescript
```

### 开发环境依赖项

在开发环境中我们将要使用Typescript编写代码。所以我们需要安装`typescript`。另外也需要安装node和express的类型声明。安装的时候带上`- D`参数来确保它是开发依赖。

```bash
yarn add -D typescript @types/express @types/node
```

- ts-node: 这个安装包是为了不用编译直接运行typescript代码，这个对本地开发太有必要了
- nodemon：这个安装包在程序代码变更之后自动监听然后重启开发服务。搭配`ts-node`模块就可以做到编写代码及时生效。

因此这两个依赖都是在开发的时候需要的，而不需编译进生产环境的。

```bash
yarn add -D ts-node nodemon
```

### 配置Typescript文件

```bash
touch tsconfig.json
```

