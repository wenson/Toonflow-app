# Docker 部署说明

本页基于仓库内的 `docker/` 配置说明 Docker 镜像的行为与注意点。

## 相关文件

- `docker/Dockerfile`：在线构建（会 clone GitHub/Gitee）
- `docker/docker-compose.yml`：在线部署（推荐）
- `docker/docker-compose.local.yml`：本地源码构建测试

## 在线部署（仓库拉取 + 构建镜像）

```bash
docker-compose -f docker/docker-compose.yml up -d --build
```

构建参数（在 `docker/Dockerfile` 与 compose 中声明）：

- `GIT`：`github`/`gitee`
- `TAG`：指定 tag
- `BRANCH`：指定分支

优先级：`TAG` > `BRANCH` > 最新 tag > 默认分支。

## 服务形态（镜像内部）

`docker/Dockerfile` 的运行阶段做了两件事：

- Nginx：服务 `scripts/web` 的静态页面（监听容器内 80）
- PM2：启动后端 `build/app.js`（容器内 60000）

## 端口映射说明

以 `docker/docker-compose.yml` 为准：

- `"60000:60000"`：固定映射后端端口
- `"80"`：未指定宿主机端口，Docker 会随机分配一个宿主机端口（用于 Nginx 静态页面）

## 数据持久化（重要）

当前后端在“非 Electron 模式”下会把数据写到容器工作目录（`process.cwd()`，即 `/app`）：

- SQLite：`/app/db.sqlite`
- uploads：`/app/uploads/`
- 日志：`/app/logs/app.log`

而现有 compose 的默认 volume 仅挂载了 `../logs:/var/log`（注意：这并不等同于挂载 `/app/logs`）。

如果你需要持久化数据，建议在 compose 里显式挂载 `/app/db.sqlite`、`/app/uploads`、`/app/logs`（以代码路径为准）。

