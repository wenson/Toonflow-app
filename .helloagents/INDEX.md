# Toonflow-app 知识库

> 本文件是知识库的入口点

## 快速导航

| 需要了解 | 读取文件 |
|---------|---------|
| 项目概况、技术栈、开发约定 | [context.md](context.md) |
| 模块索引 | [modules/_index.md](modules/_index.md) |
| 某个模块的职责和接口 | [modules/{模块名}.md](modules/{模块名}.md) |
| 项目变更历史 | [CHANGELOG.md](CHANGELOG.md) |
| 历史方案索引 | [archive/_index.md](archive/_index.md) |
| 当前待执行的方案 | [plan/](plan/) |
| 历史会话记录 | [sessions/](sessions/) |

## 模块关键词索引

> AI 读取此表即可判断哪些模块与当前需求相关，按需深读。

| 模块 | 关键词 | 摘要 |
|------|--------|------|
| electron | Electron, BrowserWindow, 主进程 | 桌面壳（主进程）与服务启动/关闭 |
| server | Express, JWT, uploads, app.ts | 本地 API 服务入口与通用中间件 |
| core-router | generateRouter, fast-glob, router.ts | 基于 `src/routes/**` 生成聚合路由入口 |
| routes | src/routes, REST, websocket | 业务 API 路由与控制器实现 |
| agents | AI, outlineScript, storyboard | AI 生成流程（大纲/剧本/分镜等） |
| db | knex, sqlite, initDB, database.d.ts | 数据库连接、初始化与类型生成 |

## 知识库状态

```yaml
kb_version: 2.2.5
最后更新: 2026-02-14 10:35
模块数量: 6
待执行方案: 0
```

## 读取指引

```yaml
启动任务:
  1. 读取本文件获取导航
  2. 读取 context.md 获取项目上下文
  3. 检查 plan/ 是否有进行中方案包

任务相关:
  - 涉及特定模块: 读取 modules/{模块名}.md
  - 需要历史决策: 搜索 CHANGELOG.md → 读取对应 archive/{YYYY-MM}/{方案包}/proposal.md
  - 继续之前任务: 读取 plan/{方案包}/*
```
