---
name: 实体类结构对比
description: 将项目中实体类定义与 `dev-manager` 中的数据库表结构进行对比，找出有差异的实体类，并生成差异报告。
---

## 配置读取
- 从  `.claude/settings.local.json` 中读取 `devManagerPath`（dev-manager 仓库根目录）。

## 对比范围
- 如果用户指定了一个或多个实体类名（如 `UserEntity`），只对比这些类；否则扫描项目下所有实体类。

## 表名映射规则
1. 优先读取实体类上的 `@Table(name = "xxx")` 或 `@Document(collection="xxx")` 注解。
2. 若无注解，按下划线小写转换：`UserEntity` → `user`，`OrderDetail` → `order_detail`。
3. 若仍找不到，在 `catalog.md` 中按实体类简单名搜索。

## 对比维度
- 字段存在性（实体类多字段或少字段）
- 字段数据类型（注意不同数据库的类型别名）
- 字段长度/精度（如 varchar(255) vs varchar(100)）
- 主键标识（是否为主键字段）

## 输出格式
以 Markdown 表格输出，示例：
| 实体类 | 表名 | 差异类型 | 字段名 | 实体定义 | 表定义 | 建议修复 |
|--------|------|----------|--------|----------|--------|----------|
| UserEntity | t_user | 缺少字段 | email | - | varchar(255) NOT NULL | 添加 `email` 字段 |

若无差异，输出“✅ 所有实体类与数据库结构一致”。

## 错误处理
- 若 `devManagerPath` 无效 → 报错并停止。
- 若 `catalog.md` 不存在 → 提醒运行 `dev-manager` 的文档生成命令。
- 若实体类无对应表 → 在报告中列在“未映射实体”部分。