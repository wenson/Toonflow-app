# 数据存储与目录（SQLite / Uploads / Logs / Env）

本页聚焦“数据落盘位置”和“如何迁移/重置”，用于本地开发、排障与部署规划。

## 1) 环境变量文件（Env）

加载逻辑：`src/env.ts`

- 非 Electron：`{repo}/env/.env.{NODE_ENV}`
- Electron：`{userData}/env/.env.{NODE_ENV}`

若文件不存在，会按默认内容自动创建（当前仅包含 `NODE_ENV/PORT/OSSURL`）。

## 2) SQLite 数据库

初始化/修复逻辑：

- 建表与默认数据：`src/lib/initDB.ts`
- 结构修复/补丁：`src/lib/fixDB.ts`

数据库文件路径（`src/utils/db.ts`）：

- 非 Electron：`{repo}/db.sqlite`
- Electron：`{userData}/db.sqlite`

开发环境额外行为：

- `NODE_ENV=dev` 时会尝试生成/更新 `src/types/database.d.ts`（同文件中 `initKnexType`）

## 3) Uploads（本地 OSS）

实现：`src/utils/oss.ts`

根目录：

- 非 Electron：`{repo}/uploads`
- Electron：`{userData}/uploads`

静态服务：

- `src/app.ts` 会将上述目录作为静态目录 `express.static(rootDir)` 挂载到根路径
- 访问 URL 由 `process.env.OSSURL` 决定（默认 `http://127.0.0.1:60000/`）

安全性：

- `oss.ts` 使用 `is-path-inside` 做路径校验，防止目录穿越

## 4) 日志（logs/app.log）

实现：`src/logger.ts`

- 非 Electron：`{repo}/logs/app.log`
- Electron：`{userData}/logs/app.log`

日志会劫持 `console.*` 以及 stdout/stderr，以捕获 `morgan` 等直接输出的内容。

## 5) 高风险“清理类”接口（谨慎使用）

以下接口属于“数据破坏性操作”，仅建议在开发/测试环境使用：

- 清空数据库：`POST /other/clearDatabase`（见 `src/routes/other/clearDatabase.ts`）
  - 行为：调用 `initDB(db, true)` 进行强制初始化（会重建/重置表）
- 删除项目目录：`POST /other/deleteAllData`（见 `src/routes/other/deleteAllData.ts`）
  - 行为：遍历 `t_project` 的 `id`，对每个 id 执行 `u.oss.deleteDirectory(String(id))`
  - 注意：当前实现并未真正清空数据库表数据（`initDB(db, true)` 被注释掉了），但响应文案仍为“清空数据库成功”，以代码行为为准

