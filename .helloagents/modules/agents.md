# agents

## 职责

封装与“AI 生成流程”相关的能力，包括但不限于：
- 大纲/剧本生成（outlineScript）
- 分镜/镜头脚本等生成（storyboard）
- 与多家模型提供商的适配与调用（见 utils/ai）

核心目录（示例）：
- `src/agents/`
  - `outlineScript/`
  - `storyboard/`

## 行为规范

### 生成流程封装
**条件**: 业务路由触发生成任务
**行为**: agents 内部组织提示词/模型调用/结果结构化
**结果**: 为 routes 提供稳定的调用接口与可复用逻辑

## 依赖关系

```yaml
依赖:
  - utils（AI 调用封装、通用工具）
被依赖:
  - routes（对外提供生成能力）
```

