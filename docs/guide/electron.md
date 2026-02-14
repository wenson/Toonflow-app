# Electron 桌面端说明

本页说明 Electron 运行时与纯 Node 运行时的差异，尤其是 **数据目录** 与 **加载路径**。

## Electron 主进程入口

- 开发态入口：`scripts/main.ts`
- 构建后入口：`build/main.js`（由 `scripts/build.ts` 产出）

开发态窗口加载的页面路径（见 `scripts/main.ts`）：

- 开发态：`scripts/web/index.html`
- 打包态：`app.getAppPath()/scripts/web/index.html`

## userData 目录（非常重要）

代码在 Electron 运行时会把数据写入 `app.getPath("userData")`，典型包括：

- 环境变量目录：`{userData}/env/.env.{NODE_ENV}`（见 `src/env.ts`）
- SQLite 数据库：`{userData}/db.sqlite`（见 `src/utils/db.ts`）
- 本地文件/OSS 根目录：`{userData}/uploads`（见 `src/utils/oss.ts` 与 `src/app.ts` 静态目录）
- 日志：`{userData}/logs/app.log`（见 `src/logger.ts`）

> `{userData}` 的具体路径与操作系统有关，最终以 Electron API 返回结果为准。

## 打包与产物

打包使用 `electron-builder`，配置在 `electron-builder.yml`：

- 输出目录：`dist/`
- `asar: true`
- 打包包含：`build/**/*`、`scripts/web/**/*`、`env/**/*`、`node_modules/**/*` 等

常用命令：

```bash
yarn build
yarn dist:win
```

> 注意：即便打包文件里包含 `env/**`，运行时实际仍以 `{userData}/env` 下的 `.env.*` 为准（缺失会自动创建），详见 `src/env.ts`。

