# express

### 安装express

```bash
npm i express
yarn add express
npx express-generator
```

### 安装依赖

```bash
yarn install	
```

### 安装swagger-ui-express

```bash
yarn add swagger-ui-express
```

### 安装swagger-jsdoc

```bash
yarn add swagger-jsdoc
```

### 安装nodemon

```bash
yarn add nodemon
```



## 安装接口文档依赖

### 安装swagger-jsdoc

```bash
yarn add swagger-jsdoc
```

安装swagger-ui-express

```bash
yarn add swagger-ui-express
```

### app.js调用

```bash
require("./swagger")(app);
```

### 配置swagger注释

```bash
/**,
 * @swagger
 * /api/baseCoinBalance:
 *    post:
 *      tags:
 *      - 平台币    #接口分类
 *      summary: 测试1   #接口备注
 *      description: 测试   #接口描述
 *      consumes:
 *      - "application/json"    #接口接收参数方式
 *      requestBody:    #编写参数接收体
 *          required: true  #是否必传
 *          content:
 *              application/json:
 *                  schema:     #参数备注
 *                      type: object    #参数类型
 *                      properties:
 *                          fromAddress:
 *                                  type: string    #参数类型
 *                                  description: 发送者钱包地址     #参数描述
 *                  example:        #请求参数样例。
 *                      fromAddress: "string"
 *      responses:  #编写返回体
 *        1000:     #返回code码
 *          description: 获取底层币余额成功     #返回code码描述
 *          content:
 *              application/json:
 *                  schema:
 *                      type: object
 *                      properties:
 *                          res_code:   #返回的code码
 *                              type: string
 *                              description: 返回code码
 *                          res_msg:    #返回体信息。***注意写的位置一定要和res_code对齐。
 *                               type: string   #返回体信息类型
 *                               description: 返回信息
 *        1001:
 *          description: 获取底层币余额失败
 * */
```

## 跨域

使用cors

### 安装cors

```bash
yarn add cors
```



