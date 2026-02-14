# server

## 职责

提供本地 API 服务入口（Express），负责：
- 基础中间件（CORS、body/json、日志等）
- 静态文件服务（uploads 目录）
- 鉴权（JWT，tokenKey 从数据库读取）
- 挂载聚合路由（`src/router.ts`）
- 404 与统一错误处理

核心文件（示例）：
- `src/app.ts`

## 接口定义（可选）

### 公共API
| 函数/方法 | 参数 | 返回值 | 说明 |
|----------|------|--------|------|
| startServe | 无 | Promise<void> | 启动 Express 服务（非 Electron 环境会自动启动） |
| closeServe | 无 | Promise<void> | 关闭服务，便于 Electron 退出前清理 |

## 行为规范

### 路由构建（开发态）
**条件**: `NODE_ENV=dev`
**行为**: 调用 `buildRoute()` 生成/更新 `src/router.ts`
**结果**: 自动聚合 `src/routes/**` 作为路由入口

### 请求鉴权
**条件**: 非白名单路径（例如 `/other/login` 以外）
**行为**: 从 `Authorization` 或 `query.token` 解析 JWT，校验失败返回 401
**结果**: 通过校验后将解码信息挂到 `req.user`

## 依赖关系

```yaml
依赖:
  - core-router（仅 dev 下生成 router.ts）
  - routes（通过 src/router.ts 挂载）
  - db（读取 tokenKey / 使用数据持久化）
被依赖:
  - electron（主进程启动/关闭服务）
```

