# routes

## 职责

承载业务 API 路由实现，按领域分目录组织（如 video/storyboard/novel 等），并由 `src/router.ts` 聚合挂载到 Express。

核心目录（示例）：
- `src/routes/`
  - `video/`、`storyboard/`、`script/`、`outline/`、`novel/`、`project/`、`task/`、`assets/`、`setting/`、`user/`、`other/`

## 接口定义（可选）

> routes 通常以 Express Router 的形式导出：`export default router;`

## 行为规范

### 参数校验
**条件**: 路由需要校验 body/query/params
**行为**: 使用 `src/middleware/middleware.ts` 的 `validateFields()` 进行 zod 校验
**结果**: 校验失败返回 400，包含字段错误信息

## 依赖关系

```yaml
依赖:
  - middleware（参数校验）
  - db（数据读写）
  - agents（AI 生成能力）
被依赖:
  - core-router（扫描生成 router.ts）
  - server（最终挂载）
```

