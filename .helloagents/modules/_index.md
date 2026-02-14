# 模块索引

> 通过此文件快速定位模块文档

## 模块清单

| 模块 | 职责 | 状态 | 文档 |
|------|------|------|------|
| electron | Electron 主进程与窗口/服务生命周期 | 🚧 | [electron.md](./electron.md) |
| server | Express 服务入口与通用中间件 | 🚧 | [server.md](./server.md) |
| core-router | 路由聚合生成（routes → router.ts） | 🚧 | [core-router.md](./core-router.md) |
| routes | 业务 API 路由实现 | 🚧 | [routes.md](./routes.md) |
| agents | AI 生成流程与能力封装 | 🚧 | [agents.md](./agents.md) |
| db | SQLite/Knex 初始化与类型生成 | 🚧 | [db.md](./db.md) |

## 模块依赖关系

```
electron → server → routes → agents
                 ↘ utils/db
core-router → router.ts（生成文件，被 server 引用）
```

## 状态说明
- ✅ 稳定
- 🚧 开发中
- 📝 规划中

