# **MyProject**

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

```bash
pnpm add vuex@next --save
```

#### 使用方式

```tsx
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

```tsx
import router from './router'
app.use(router)
```



### **安装ElementPlus**

```bash
pnpm install element-plus
```

#### 使用方式

```tsx
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

```tsx
import { Edit,Camera } from '@element-plus/icons-vue'
app.component('edit', Edit)
app.component('camera', Camera)
```

#### 使用方式(单文件)

```tsx
import { Edit,Camera } from '@element-plus/icons-vue'
```

### 安装@types/node

```bash
pnpm add --save @types/node
```

#### 配置

```tsx
// tsconfig.json
{
  "compilerOptions": {
    "types": ["node"]
  }
}
```



### 配置vite.config.ts

这里主要配置vite服务器的打包地址,打包分解,端口号,是否自动打开浏览器,远程请求地址代理目标,目录别名,全局样式配置等等

```tsx
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import { loadEnv } from "vite";
import path from 'path'
// https://vitejs.dev/config/
// export default defineConfig({
//     plugins: [vue()],
//     css: {
//         // preprocessorOptions: {
//         //     sass: {
//         //         additionalData: `$injectedColor: orange;`,
//         //     },
//         // },
//     },
// });

export default (command, mode) => {
    return defineConfig({
        plugins: [vue()],
        server: {
            host: '127.0.0.1',
            port: Number(loadEnv(mode, process.cwd()).VITE_APP_PORT),
            // port: 9999,
            strictPort: true, // 端口被占用直接退出
            open: true, // 在开发服务器启动时自动在浏览器打开
            https: false, // 默认用http方式
            proxy: { // 代理配置
                '/api': {
                    // target: '',
                    target: loadEnv(mode, process.cwd()).VITE_APP_URL,
                    changeOrigin: true, //跨域配置
                    rewrite: (path) => path.replace(/^\/api/, '')
                },
                // 正则写法
                // '^/fallback/.*': {
                //     target: '',
                //     changeOrigin: true, //跨域配置
                //     rewrite: (path) => path.replace(/^\/api/, '^/fallback/')
                // }
            },
            hmr: {
                overlay: false // 屏蔽服务器报错
            }
        },
        resolve: { //解析项目文件导入路径
            alias: {
                '@': path.resolve(__dirname, './src')
            }

        },
        css: {

        },
        build: { // 分解打包配置
            chunkSizeWarningLimit: 1500,
            rollupOptions: {
                output: {
                    manualChunks(id) {
                        if (id.includes('node_modules')) {
                            return id.toString().split('node_modules/')[0].toString()
                        }
                    }
                }
            }
        }
    });
};
```

### .env.环境变量配置和读取

环境变量建议放到根目录下,方便vite.config.ts读取和使用

```tsx
.evn.production //生产环境配置文件
.evn.development //开发环境配置文件

// .evn.production
# 开发环境
VITE_APP_TITLE = '生产'
VITE_APP_PORT = 8088
# 请求接口
VITE_APP_BASE_URL = 'http://192.168.1.118:80'

// .evn.development
# 开发环境
VITE_APP_TITLE = '开发'
VITE_APP_PORT = 8089
# 请求接口
VITE_APP_BASE_URL = 'http://192.168.1.118:80'

//  env.d.ts
interface ImportMetaEnv {
    VITE_APP_TITLE: string
	VITE_APP_PORT: number
	VITE_APP_BASE_URL: string
}

```

**写变量时一定要以VITE开头,才能暴露给外部读取**

