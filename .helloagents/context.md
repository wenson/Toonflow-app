# 项目上下文

## 1. 基本信息

```yaml
名称: Toonflow-app
描述: Toonflow 是一款 AI 短剧漫剧工具，能够利用 AI 技术将小说自动转化为剧本，并结合 AI 生成的图片和视频，实现高效的短剧创作。
类型: Electron 桌面应用（内置 Express 本地服务）
状态: 开发中
```

## 2. 技术上下文

```yaml
语言: TypeScript（Node.js）
框架: Electron + Express
包管理器: Yarn（Classic / v1）
构建工具: esbuild + tsx + electron-builder
```

### 主要依赖

| 依赖 | 版本 | 用途 |
|------|------|------|
| electron | ^40.0.0 | 桌面壳与主进程运行时 |
| express | ^5.2.1 | 本地 API 服务（HTTP） |
| express-ws | ^5.0.2 | WebSocket 支持（挂载到 Express） |
| knex | ^3.1.0 | 数据库访问层（SQL builder） |
| sqlite3 / better-sqlite3 | ^5.1.7 / ^12.6.2 | SQLite 驱动（构建时 external） |
| ai + @ai-sdk/* | ^6.0.67 等 | AI 模型调用与多厂商适配 |
| zod | ^4.3.5 | 参数校验（中间件）与数据校验 |

## 3. 项目概述

### 核心功能（从目录结构推断）

- 小说/项目相关：`src/routes/novel`、`src/routes/project`
- 大纲/剧本/分镜：`src/routes/outline`、`src/routes/script`、`src/routes/storyboard`
- 视频相关：`src/routes/video`
- 资产/配置/用户：`src/routes/assets`、`src/routes/setting`、`src/routes/user`

### 项目边界（当前实现倾向）

```yaml
范围内:
  - 本地启动 Electron 主进程 + 本地 Express API 服务
  - SQLite 本地持久化（db.sqlite 位于 Electron userData 或工作目录）
范围外:
  - 远程部署的多实例后端（当前代码默认本地运行形态）
```

## 4. 开发约定

### 代码规范（从配置推断）

```yaml
TypeScript: strict=true（tsconfig.json）
路径别名: "@/*" -> "src/*"
模块格式: CommonJS（编译目标）
```

### 错误处理

```yaml
HTTP: Express 统一错误中间件返回 err.status 或 500
参数校验: zod 校验失败返回 400（见 src/middleware/middleware.ts）
```

### 测试要求（现状）

```yaml
测试框架: 未发现 Jest/Vitest 等单测框架依赖
现有脚本: yarn test = cross-env NODE_ENV=prod node build/app.js（偏运行时检查）
```

### Git规范

```yaml
分支策略: 未在仓库内发现明确约定（可补充到 docs/ 或本文件）
提交格式: 未在仓库内发现明确约定
```

## 5. 当前约束（源自历史决策）

> 暂无归档方案包可追溯（首次初始化知识库）

## 6. 已知技术债务（可选）

> 暂未系统梳理，可在后续任务中逐步补全

