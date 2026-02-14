# 后端概览（Express）

## 启动入口与生命周期

- 服务入口：`src/app.ts`
- 非 Electron 运行时：文件末尾会直接调用 `startServe()`
- Electron 运行时：由 `scripts/main.ts` 在 `app.whenReady()` 后调用 `startServe()`

服务关闭：`src/app.ts` 导出 `closeServe()`，Electron 在 `before-quit` 事件里调用。

## 中间件与能力

`src/app.ts` 中主要启用：

- `morgan`：HTTP 日志（同时会被 `src/logger.ts` 劫持写入文件）
- `cors({ origin: "*" })`
- `express.json({ limit: "100mb" })` 与 `express.urlencoded({ limit: "100mb" })`
- `express-ws`：启用 WebSocket 支持（具体 WS 路由以代码为准）

## 静态文件（uploads 作为“本地 OSS”）

服务会把 `uploads/` 作为静态目录并挂载在根路径（见 `src/app.ts`）：

- 非 Electron：`{repo}/uploads`
- Electron：`{userData}/uploads`

配套的文件读写封装在 `src/utils/oss.ts`，并通过 `process.env.OSSURL` 拼接对外访问 URL。

## 路由组织方式

- 路由文件目录：`src/routes/**`
- 路由聚合入口：`src/router.ts`（自动生成）
- 生成器：`src/core.ts`

路由路径基本规则：

- `src/routes/novel/addNovel.ts` → `/novel/addNovel`
- `src/routes/index/index.ts` → `/index`（注意：路由生成器对 `index` 有特殊处理）

详细见 [路由注册与自动生成](./router-generation.md)。

