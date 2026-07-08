---
name: 任务驱动开发
description: 在全栈开发模式中，这个技能描述了开发者如何进行开发任务的流程管理。
---

> 本文件为公共模板源，初始化项目时会被复制到前端/后端项目各自的 skills 目录下，复制时将 `{{taskFile}}` 占位符替换为 `frontend.md`（前端空间）或 `backend.md`（后端空间）。修改本文件后，需重新初始化或同步到两个目标空间。

## 流程
1. 读取项目根目录下的 `setting.local.json`，找到项目管理根目录 `devManagerPath`。

2.  读取 `<devManagerPath>/dev-tasks/{{taskFile}}`，找出需要进行开发的任务，查看任务描述。若无任务，输出“无待开发任务”并结束。

3. 更新 `<devManagerPath>/dev-tasks/{{taskFile}}` 中对应任务下的状态为开发中。

4. 开始开发。

5. 开发完成后，更新 `<devManagerPath>/dev-tasks/{{taskFile}}` 中对应任务下的状态为待测试。
