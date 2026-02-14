# electron

## 职责

负责 Electron 主进程生命周期管理，创建主窗口，并在应用就绪时启动内置的本地服务；在退出前负责关闭服务。

核心文件（示例）：
- `scripts/main.ts`：创建 `BrowserWindow`，启动/关闭服务

## 接口定义（可选）

### 公共API
| 函数/方法 | 参数 | 返回值 | 说明 |
|----------|------|--------|------|
| createMainWindow | 无 | void | 创建主窗口并加载 `scripts/web/index.html` |
| app.whenReady().then | 无 | Promise<void> | 应用就绪后启动窗口与服务 |
| app.on("before-quit") | event | void | 退出前关闭服务（调用 `closeServe()`） |

## 行为规范

### 启动流程
**条件**: Electron `app` 就绪
**行为**: 创建窗口 → 调用 `startServe()` 启动本地服务
**结果**: 桌面应用可用，API 服务监听本地端口

## 依赖关系

```yaml
依赖:
  - server（src/app.ts 的 startServe/closeServe）
被依赖:
  - build（electron-builder 打包产物 build/main.js）
```

