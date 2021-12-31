# **VITE创建项目包**

### 安装项目包

```bash
pnpm init vite-app [project_name]
```

### 进入项目

```bash
cd [project_name]
```

### 安装依赖

```bash
pnpm install	
```

### 安装typescript

```bash
pnpm add -D typescript
```

### 安装vuex

```tsx
pnpm add vuex@next --save
```

#### 使用方式

```bash
import {store, key} from './store'
app.use(store, key)
```



### 安装vue-router

```tsx
pnpm add vue-router@4
import router from './router'
app.use(router)
```

#### 使用方式

```
import router from './router'
app.use(router)
```



### **安装ElementPlus**

```tsx
pnpm install element-plus
```

#### 使用方式

```bash
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
app.use(ElementPlus)		
```



### **安装svg-icon**

 **Font Icon** has been removed, since version `1.2.0-beta.1`

```bash
pnpm install @element-plus/icons-vue
```

#### 使用方式(全局)

```bash
import { Edit,Camera } from '@element-plus/icons-vue'
app.component('edit', Edit)
app.component('camera', Camera)
```

#### 使用方式(单文件)

```bash
import { Edit,Camera } from '@element-plus/icons-vue'
```

### 配置vite.config.ts

这里主要配置vite服务器的打包地址,打包分解,端口号,是否自动打开浏览器,远程请求地址代理目标,目录别名,全局样式配置等等

```

```

