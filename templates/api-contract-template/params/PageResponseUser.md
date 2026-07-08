# `PageResponseUser`

用户分页响应数据

## 属性

| 名称 | 类型 | 描述 |
|------|------|------|
| `page` | integer(int32) | 当前页码 |
| `pageSize` | integer(int32) | 每页条数 |
| `total` | integer(int64) | 总记录数 |
| `list` | array< [User](./User.md) > | 当前页数据列表 |

**必需字段:** `page`, `pageSize`, `total`, `list`
