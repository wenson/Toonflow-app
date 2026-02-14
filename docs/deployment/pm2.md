# PM2 / 云端部署说明

本页用于“在服务器上以 Node 方式运行后端”，不包含 Electron GUI。

## 运行前提

- Node.js：建议 24.x（与 Docker 镜像基线一致）
- Yarn：用于安装依赖与构建

## 构建与启动

在服务器上（以 `/opt/Toonflow-app` 为例）：

```bash
yarn install
yarn build
```

构建后入口为：

- `build/app.js`

你可以直接运行：

```bash
NODE_ENV=prod PORT=60000 node build/app.js
```

## PM2 配置示例

示例与根 `README.md` 中的 `pm2.json` 保持一致思路（端口与 `OSSURL` 需按实际修改）：

```json
{
  "name": "toonflow-app",
  "script": "build/app.js",
  "instances": "max",
  "exec_mode": "cluster",
  "env": {
    "NODE_ENV": "prod",
    "PORT": 60000,
    "OSSURL": "http://127.0.0.1:60000/"
  }
}
```

## 运行时数据目录

非 Electron 模式下，后端会使用 `process.cwd()` 作为根目录写入：

- `db.sqlite`
- `uploads/`
- `logs/app.log`
- `env/.env.prod`（缺失会自动创建）

因此建议在部署时明确工作目录，并将上述路径纳入备份与持久化策略。

