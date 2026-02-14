# 常见问题排查

本页按“症状 → 可能原因 → 处理方式”的形式整理。

## 1) 服务起不来 / 端口被占用

症状：

- 控制台报错 `EADDRINUSE`

处理：

- 修改 `env/.env.dev` 或 `env/.env.prod` 中的 `PORT`
- 或结束占用该端口的进程

## 2) 登录成功但其它接口一直 401

可能原因：

- 未携带 `Authorization` header
- header 中 `Bearer ` 前缀格式不对
- 数据库里的 `t_setting.tokenKey` 发生变化导致旧 token 失效

处理：

- 重新调用 `POST /other/login` 获取新 token

## 3) Docker 运行后页面能打开，但 API 不通 / 静态页端口不确定

说明：

- `docker/docker-compose.yml` 使用 `"80"`（未指定宿主机端口）会随机分配宿主机端口
- 后端端口使用 `"60000:60000"` 固定映射

处理：

- 用 `docker ps` 查看宿主机实际映射的静态页端口
- 或在 compose 中改为 `8080:80` 这类固定映射

## 4) 数据/上传文件“重启就没了”（Docker）

原因：

- 数据默认写在容器 `/app` 下（`db.sqlite`、`uploads/`、`logs/`），未做 volume 挂载会随容器生命周期变化

处理：

- 按 [Docker 部署说明](./deployment/docker.md) 挂载需要持久化的目录/文件

## 5) Electron 与纯 Node 的数据不在同一个地方

说明：

- Electron 模式使用 `app.getPath("userData")` 存放 `env/db/uploads/logs`
- 纯 Node 模式使用项目根目录（`process.cwd()`）

处理：

- 先确认当前运行形态，再去对应目录查找 `db.sqlite`、`uploads/`、`logs/app.log`

