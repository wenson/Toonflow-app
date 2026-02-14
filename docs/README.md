# 开发者文档（docs/）

本目录用于存放 **Toonflow-app** 的开发者文档，按主题拆分为多页，便于维护与检索。

> 真实性基准：当文档与代码行为不一致时，以代码为准，并应同步更新文档。

## 文档地图

**上手与开发**

- [快速上手（本地跑起来）](./guide/quick-start.md)
- [本地开发指南（脚本/调试/生成文件）](./guide/local-development.md)
- [Electron 桌面端说明（路径/打包/运行差异）](./guide/electron.md)

**后端（Express）**

- [后端概览（入口/中间件/静态文件）](./backend/overview.md)
- [认证与登录（Token 规则）](./backend/auth.md)
- [路由注册与自动生成（src/core.ts → src/router.ts）](./backend/router-generation.md)

**数据与文件**

- [数据存储与目录（SQLite/Uploads/Logs/Env）](./data/storage.md)

**部署**

- [Docker 部署说明（当前 compose 的注意点）](./deployment/docker.md)
- [PM2/云端部署说明](./deployment/pm2.md)

**排障**

- [常见问题排查](./troubleshooting.md)

## 维护约定

- 文档统一使用中文编写（专有名词、代码标识符保持原样）。
- 新增页面时，优先在本页补充入口链接。
- 对于“会被代码自动生成”的文件（如 `src/router.ts`、`src/types/database.d.ts`），文档需要明确标注“不要手改”的原因与刷新方式。

