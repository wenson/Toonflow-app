# 认证与登录（Token 规则）

## 认证中间件位置

见 `src/app.ts`：

- 白名单路径：`/other/login`
- 其余路径：必须提供 Token，否则返回 `401`

Token 获取来源（按优先级）：

1. Header：`Authorization`
2. Query：`token`

并会对 `Bearer ` 前缀做一次剥离后再校验。

## 登录接口

路由文件：`src/routes/other/login.ts`

- `POST /other/login`
- Body：`{ "username": string, "password": string }`
- 成功返回：`{ token: "Bearer xxx", name, id }`

默认用户来自数据库初始化（`src/lib/initDB.ts`）：

- 用户名：`admin`
- 密码：`admin123`

## Token Secret 的来源与有效期

Token 的签名密钥来自数据库 `t_setting.tokenKey`（见 `src/routes/other/login.ts` 与 `src/app.ts`）。

- 初始化时会创建默认 `tokenKey`（`src/lib/initDB.ts`）
- 登录时会用该 `tokenKey` 生成 token
- 校验时也用同一 `tokenKey` 校验

因此：

- 如果 `tokenKey` 被重置/变更，旧 Token 会整体失效
- 过期时间由 `login.ts` 中的 `expiresIn` 决定（当前实现为 `"180Days"`）

