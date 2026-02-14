# core-router

## 职责

将 `src/routes/**/*.ts` 按文件路径规则转换成 Express 路由路径，并生成 `src/router.ts`，由服务入口统一挂载。

核心文件（示例）：
- `src/core.ts`：扫描 routes 并生成 `src/router.ts`

## 接口定义（可选）

### 公共API
| 函数/方法 | 参数 | 返回值 | 说明 |
|----------|------|--------|------|
| generateRouter | 无 | Promise<void> | 生成/更新 `src/router.ts`（带 hash，避免不必要写入） |

## 行为规范

### 路由路径规则
**条件**: 路由文件位于 `src/routes/**`
**行为**: 基于文件名/目录名生成路径（支持 `[id]` → `:id`，`[...rest]` → `*`，`index` → `/`）
**结果**: `src/router.ts` 内容稳定、可复现

## 依赖关系

```yaml
依赖:
  - src/routes（输入源）
被依赖:
  - server（dev 环境下调用生成逻辑，并 import router.ts 挂载）
```

