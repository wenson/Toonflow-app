# 快速上手（本地跑起来）

本页面向“把服务先跑起来”的开发者，尽量少前置知识。

## 1) 环境准备

- Node.js：建议使用 **24.x**（Dockerfile 使用 `node:24-alpine` 作为构建/运行基线）。
- Yarn：项目根 `package.json` 的 `packageManager` 为 `yarn@1.0.0`（Yarn Classic）。

## 2) 安装依赖

在项目根目录执行：

```bash
yarn install
```

## 3) 启动后端服务（不含 Electron GUI）

```bash
yarn dev
```

启动成功后默认监听 `PORT`（默认 `60000`），控制台会输出类似：

```
[服务启动成功]: http://localhost:60000
```

## 4) 登录与鉴权（最小验证）

服务端除 `/other/login` 外，其它 API 默认都需要 Token（见 `src/app.ts` 的中间件）。

默认账号来自数据库初始化逻辑（见 `src/lib/initDB.ts`）：

- 用户名：`admin`
- 密码：`admin123`

登录接口：

- `POST /other/login`
- Body：`{ "username": "...", "password": "..." }`
- 返回：`token` 字段（形如 `Bearer xxx`）

之后请求在 Header 中带上：

- `Authorization: Bearer xxx`

或使用 Query 参数：

- `?token=Bearer%20xxx`

## 5) 第一次运行会生成哪些文件？

根据代码逻辑（`src/env.ts`、`src/utils/db.ts`、`src/logger.ts`、`src/utils/oss.ts`），非 Electron 运行时会在项目根目录创建/使用：

- `env/.env.dev` 或 `env/.env.prod`（缺失会自动创建）
- `db.sqlite`
- `uploads/`（静态文件目录，同时作为“本地 OSS”根目录）
- `logs/app.log`

> Electron 运行时这些文件会改为写入 Electron 的 `userData` 目录，详见 [Electron 桌面端说明](./electron.md)。

