# <表名> 变更历史

> 本文件按时间倒序记录该表/集合的所有 schema 变更。每条记录通过 **dev-task ID**（`<模块名称>-<YYYYMMDD>-<序号>`）与对应的模块变更日志双向引用。
> 模块变更日志位置：`/architecture/module/<模块名称>/change-log/<YYYYMMDD>-<序号>.md`

---

## <YYYYMMDD>-<序号> | <dev-task ID>

- **变更类型**：新增表 / 修改字段 / 删除字段 / 修改索引 / 删除索引 / 修改约束 / 废弃表
- **破坏性变更**：否 / 是（说明破坏点：如删除字段、改字段类型、改唯一约束等）
- **变更详情**：
  - <字段或索引级的具体变更，逐条列出，例如：>
  - 新增字段 `avatar_url`（varchar(255)，nullable）
  - 修改字段 `status` 默认值 `0` → `1`
  - 新增索引 `idx_users_status`（`status`）
- **迁移说明**：无 / <如需数据迁移，写明迁移步骤与回滚方案>
- **关联模块变更日志**：[/architecture/module/<模块名称>/change-log/<YYYYMMDD>-<序号>.md](../../../module/<模块名称>/change-log/<YYYYMMDD>-<序号>.md)

---

<!-- 后续变更按时间倒序在此分隔线之上追加，最新在最上 -->
