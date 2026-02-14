# db

## 职责

负责 SQLite 数据库的：
- 路径选择（Electron userData 或工作目录）
- 连接与初始化（Knex）
- 数据结构修复/迁移（`fixDB`）
- 开发态数据库类型生成（生成 `src/types/database.d.ts`）

核心文件（示例）：
- `src/utils/db.ts`
- `src/lib/initDB.ts`
- `src/lib/fixDB.ts`
- `src/types/database.d.ts`（自动生成）

## 接口定义（可选）

### 公共API
| 函数/方法 | 参数 | 返回值 | 说明 |
|----------|------|--------|------|
| dbClient | tableName | QueryBuilder | `dbClient("t_xxx")` 形式的表访问器（带类型） |
| db | — | Knex | 原始 knex 实例 |

## 行为规范

### 初始化与修复
**条件**: 模块加载（启动时）
**行为**: `initDB(db)` → `fixDB(db)`；dev 环境下可能生成类型
**结果**: 数据库可用且与期望结构一致

## 依赖关系

```yaml
依赖:
  - knex/sqlite3（运行时依赖）
  - lib/initDB、lib/fixDB
被依赖:
  - server / routes（读取配置与业务数据）
```

