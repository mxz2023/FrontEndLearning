# vue3-ts-cms

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Type Support for `.vue` Imports in TS

TypeScript cannot handle type information for `.vue` imports by default, so we replace the `tsc` CLI with `vue-tsc` for type checking. In editors, we need [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin) to make the TypeScript language service aware of `.vue` types.

If the standalone TypeScript plugin doesn't feel fast enough to you, Volar has also implemented a [Take Over Mode](https://github.com/johnsoncodehk/volar/discussions/471#discussioncomment-1361669) that is more performant. You can enable it by the following steps:

1. Disable the built-in TypeScript Extension
   1. Run `Extensions: Show Built-in Extensions` from VSCode's command palette
   2. Find `TypeScript and JavaScript Language Features`, right click and select `Disable (Workspace)`
2. Reload the VSCode window by running `Developer: Reload Window` from the command palette.

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Type-Check, Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

### Video 215~xxx

- 215 初始化工程：npm init vue@latest
- 216 EditorConfig、Prettier、ESLint集成、其他第三库集成

  - 集成editorconfig配置（有助于不同IDE编辑器上处理同一项目多个开发保持一致的编码风格）

    ```
      # http://editorconfig.org (需要安装 EditorConfig for VS Code)

      root = true

      [*] # 表示所有文件适用
      charset = utf-8 # 设置文件字符集为 utf-8
      indent_style = space # 缩进风格（tab | space）
      indent_size = 2 # 缩进大小
      end_of_line = lf # 控制换行类型(lf | cr | crlf)
      trim_trailing_whitespace = true # 去除行尾的任意空白字符
      insert_final_newline = true # 始终在文件末尾插入一个新行

      [*.md] # 表示仅 md 文件适用以下规则
      max_line_length = off
      trim_trailing_whitespace = false

    ```

  - prettier配置

    - 创建.prettier、.prettierignore，在里面进行配置

      ```
        {
          "useTabs": false,
          "tabWidth": 2,
          "printWidth": 80,
          "singleQuote": true,
          "trailingComma": "none",
          "semi": false
        }
      ```

      ```
          /dist/*
        .local
        .output.js
        /node_modules/**

        **/*.svg
        **/*.sh

        /public/*
      ```

    - 安装vscode插件、下载prettier npm包(在package.json中scripts添加："prettier": "prettier --write .")、设置vscode的default formatter为prettier

  - eslint配置

    - 让eslint读取prettier的代码格式风格，安装
      `npm install -D eslint-plugin-prettier`
      `npm install -D eslint-config-prettier`
    - 在.eslintrc配置文件中的extends中添加
      `'plugin:prettier/recommended'`
    - vscode中eslint配置
      ```
        "eslint.lintTask.enable": true,
        "eslint.alwaysShowStatus": true,
        "eslint.validate": [
          "javascript",
          "javascriptreact",
          "typescript",
          "typescriptreact"
        ],
        "editor.codeActionsOnSave": {
          "source.fixAll.eslint": true
        },
      ```

  - css重置
    - `npm install normalize.css`
    - Preprocessor dependency "less" not found. Did you install it? Try `npm install -D less`.
      `npm install -D less`
  - 路由配置

    - `npm install vue-router`
    - router/index.ts

      ```
        import { createRouter, createWebHashHistory } from 'vue-router'

        const router = createRouter({
          history: createWebHashHistory(),
          routes: [
            {
              path: '/',
              redirect: '/main'
            },
            {
              path: '/main',
              component: () => import('../views/Main/Main.vue')
            },
            {
              path: '/login',
              component: () => import('../views/Login/Login.vue')
            },
            {
              path: '/:pathMatch(.*)',
              component: () => import('../views/NotFound/NotFound.vue')
            }
          ]
        })

        // 导航守卫

        export default router

      ```

  - 状态管理(vuex/pinia; 推荐pinia)
    - `npm install pinia`
  - 网络请求封装(axios)
    - `npm install axios`
  - 环境区分

    - import.meta.env.MODE
    - import.meta.env.DEV/import.meta.env.PROD/import.meta.env.SSR
    - 通过环境变量文件 (内容以VITE\_开头才会被vite加载)

      - .env: 所有情况都会加载
      - .env.[mode]:`.env.development`,`.env.production`; 指定模式加载
      - `.env.[mode].local`/`.env.local`: 会被git忽略

      ```
        VITE_xxx
      ```

- 217 Element-Plus/图标/登录页

  - Element-Plus集成
    - [按需导入](https://element-plus.org/zh-CN/guide/quickstart.html#%E6%8C%89%E9%9C%80%E5%AF%BC%E5%85%A5)
      - 安装:
        - `npm install element-plus`
        - `npm install -D unplugin-vue-components unplugin-auto-import`
      - vite配置
      - tsconfig.json配置
        ```
          {
            "include": ["env.d.ts", "src/**/*", "src/**/*.vue", "auto-imports.d.ts", "components.d.ts"]
          }
        ```
  - 图标集成 (`npm install @element-plus/icons-vue`)

    ```
      import * as ElementPlusIconsVue from '@element-plus/icons-vue'

        const app = createApp(App)
        for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
          app.component(key, component)
        }
    ```

  - 引入ElMessage、ElLoading样式

    - 单独import
      ```
        import 'element-plus/theme-chalk/el-message.css'
        import 'element-plus/theme-chalk/el-loading.css'
      ```
    - 通过插件`npm install -D vite-plugin-style-import`、`npm install -D consola`

      ```
        import { createStyleImportPlugin, ElementPlusResolve } from 'vite-plugin-style-import'

        createStyleImportPlugin({
          resolves: [ElementPlusResolve()],
          libs: [
            {
              libraryName: 'element-plus',
              esModule: true,
              resolveStyle: (name: string) => {
                return `element-plus/theme-chalk/${name}.css`
              }
            }
          ]
        })
      ```

- 218 登录逻辑、类型编写
- 219 路由守卫、退出登录、获取用户信息
  - RBAC: role based access control (基于角色的访问控制)
    登录 -> 获取角色 -> 获取权限
    https://transform.tools/json-to-typescript
