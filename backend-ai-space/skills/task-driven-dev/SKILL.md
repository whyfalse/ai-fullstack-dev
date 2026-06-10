---
name: 任务驱动开发
description: 在全栈开发模式中，这个技能描述了开发者如何进行开发任务的流程管理。
---

## 流程
1. 读取本地设置 ，找到项目管理根目录`devManagerPath`。
	
2.  读取 `<devManagerPath>/dev-tasks/backend.md`，找出需要进行开发的任务，查看任务描述。若无任务，输出“无待开发任务”并结束。
    
3. 更新 `<devManagerPath>/dev-tasks/backend.md` 中对应任务下的状态为开发中。
	
4. 开始开发。
	
5. 开发完成后，更新 `<devManagerPath>/dev-tasks/backend.md` 中对应任务下的状态为待测试。
    