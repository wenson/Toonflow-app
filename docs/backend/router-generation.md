# 路由注册与自动生成（src/core.ts → src/router.ts）

## 为什么要自动生成？

本项目将 `src/routes/**` 下的每个路由文件视为一个 Router 模块，通过自动生成的 `src/router.ts` 完成集中注册，减少手工维护 `import/app.use` 的成本。

## 生成器

- 生成器代码：`src/core.ts`
- 输出文件：`src/router.ts`
- 文件头标记：`// @routes-hash ...`

`src/core.ts` 会：

1. 用 `fast-glob` 扫描 `src/routes/**/*.ts`
2. 将文件路径映射为路由路径
3. 生成 `import routeX from "./routes/..."` 与 `app.use("...", routeX)` 列表
4. 计算 hash，若与现有 `src/router.ts` 的 hash 相同则不改写文件

## 触发时机

在 `src/app.ts` 中，只有当 `NODE_ENV=dev` 时才会先调用路由生成：

```ts
if (process.env.NODE_ENV == "dev") await buildRoute();
```

因此，开发时新增路由后通常 **重启 `yarn dev`** 就会刷新 `src/router.ts`。

## 路由路径映射规则（简化版）

以 `src/routes` 为根：

- `a/b.ts` → `/a/b`
- `a/index.ts` → `/a`（末尾 `/index` 会被折叠）
- `index.ts` → `/`
- `[id].ts` → `/:id`
- `[...all].ts` → `/*`

具体规则以 `src/core.ts` 中 `fileNameToRoutePath()` 实现为准。

## 新增一个路由（推荐流程）

1. 在 `src/routes/<模块>/` 下新增一个 `.ts` 文件，导出 `express.Router()` 实例
2. 运行/重启 `yarn dev`，让 `src/router.ts` 自动更新
3. 通过接口调用验证路径是否正确

> 不建议手工编辑 `src/router.ts`，因为它会在开发态被自动覆盖。

