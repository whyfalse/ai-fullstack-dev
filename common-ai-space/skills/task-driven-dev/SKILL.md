---
name: 任务驱动开发
description: 在全栈开发模式中，这个技能描述了开发者如何进行开发任务的流程管理。
---

> 本文件为公共模板源，初始化项目时会被复制到前端/后端项目各自的 skills 目录下，复制时将 `{{taskFile}}` 占位符替换为 `frontend.md`（前端空间）或 `backend.md`（后端空间）。修改本文件后，需重新初始化或同步到两个目标空间。

## 流程
1. 读取项目根目录下的 `setting.local.json`，找到项目管理根目录 `devManagerPath`。

2.  读取 `<devManagerPath>/dev-tasks/{{taskFile}}`，挑出状态为 `todo` 的任务，查看任务描述。若无 `todo` 任务，输出"无待开发任务"并结束。**不挑 `blocked` 任务**（串行场景下前端任务等后端完成后由用户改为 `todo`）。

3. **读取本次变更对应的变更日志**，获取本次任务的真正需求来源：
   - 从任务行中取出**所属模块**列与**关联的变更**列。
   - 关联的变更为变更ID，形如 `<任务简介>-<YYYYMMDD>-<seq>`，截取其中的 `<YYYYMMDD>-<seq>` 段。
   - 读取 `<devManagerPath>/architecture/module/<所属模块>/change-log/<YYYYMMDD>-<seq>.md`，理解本次变更具体新增/修改了哪些 API、表、交互或逻辑。
   - 变更日志是 task 级的需求来源；模块全量文档（`frontend.md` / `backend.md`）作为背景按需查阅，不作为本次任务范围的权威依据。
   - 若关联的变更为空或变更日志文件不存在，停止开发并报告"任务 `<编号>` 缺少关联的变更或对应的变更日志 `<YYYYMMDD>-<seq>.md` 未找到，请确认架构师是否已分配变更并生成变更日志"。

4. 更新 `<devManagerPath>/dev-tasks/{{taskFile}}` 中对应任务下的状态为开发中。

5. 开始开发。

6. 开发完成后，更新 `<devManagerPath>/dev-tasks/{{taskFile}}` 中对应任务下的状态为待测试。
