# 本地开发指南

本页聚焦“如何开发/调试/构建”，内容以仓库当前脚本与代码为准。

## 常用命令（package.json）

- 启动后端（仅 API）：`yarn dev`
  - `nodemon --inspect --exec tsx src/app.ts`
- 启动桌面端（后端 + Electron 窗口）：`yarn dev:gui`
  - `electronmon -r tsx scripts/main.ts`
- 类型检查：`yarn lint`
  - `tsc --noEmit`
- 构建（输出到 `build/`）：`yarn build`
  - `cross-env NODE_ENV=prod tsx scripts/build.ts`
- 打包 Electron：`yarn dist:win` / `yarn dist:mac` / `yarn dist:linux`
- AI 调试面板：`yarn debug:ai`

## `yarn dev` vs `yarn dev:gui`

| 命令 | 启动内容 | 运行形态 | 静态页面来源 |
| --- | --- | --- | --- |
| `yarn dev` | Express 后端 | Node 进程 | 不启动 Electron，不加载页面 |
| `yarn dev:gui` | Express 后端 + Electron 主进程 | 桌面应用 | 开发态加载 `scripts/web/index.html` |

Electron 主进程入口见 `scripts/main.ts`。

## 构建产物与打包

构建脚本 `scripts/build.ts` 会用 esbuild 生成：

- `build/app.js`：后端服务入口（由 `src/app.ts` 打包）
- `build/main.js`：Electron 主进程（由 `scripts/main.ts` 打包）

打包配置见 `electron-builder.yml`，输出目录默认为 `dist/`。

## 自动生成文件（不要手改）

### 1) `src/router.ts`

- 生成逻辑：`src/core.ts`
- 触发方式：开发环境启动服务时（`src/app.ts` 中 `NODE_ENV=dev` 时调用）会生成/更新 `src/router.ts`
- 识别方式：文件头包含 `// @routes-hash ...`

新增/修改路由时，按约定新增 `src/routes/**` 下的路由文件，然后重启 `yarn dev` 即可刷新路由表。

### 2) `src/types/database.d.ts`

- 生成逻辑：`src/utils/db.ts`（`NODE_ENV=dev` 时会生成类型声明）
- 识别方式：文件头包含 `// @db-hash ...` 与“该文件由脚本自动生成”

数据库表结构变化后，通常重启 `yarn dev` 就会触发重新生成（前提是相关初始化流程执行到了 `initKnexType`）。

## 前端资源（内置 web）

本仓库内置了前端静态资源：

- `scripts/web/`

Electron 开发态与 Docker 镜像都会直接使用该目录的 `index.html`（详见 `scripts/main.ts` 与 `docker/Dockerfile`）。

如果你在单独的前端仓库里开发（如 `Toonflow-web`），构建完成后需要将前端构建产物同步到本仓库的 `scripts/web/`，再运行 `yarn dev:gui` 或重新构建 Docker 镜像。

