---
name: ai-fullstack-dev
description: 创建一个AI全栈开发项目
---

请严格按照下面的流程进行创建：

> 说明：本仓库中的 `dev-manager-ai-space/`、`frontend-ai-space/`、`backend-ai-space/`、`common-ai-space/` 四个目录以及 `需求分析师Agent.md`、`架构师Agent.md`、`前端开发者Agent.md`、`后端开发者Agent.md`、`SKILL.md` 是**模板源文件**，仅用于初始化时读取，不需要复制到新项目。新项目只需复制 `templates/` 目录，以及各 `*-ai-space/rules/` 和 `*-ai-space/skills/` 下的内容到对应位置。`common-ai-space/skills/` 下为前后端共享的技能模板，初始化时需复制到前端与后端两个空间，并按下方步骤替换占位符。

- 让用户确认是否使用当前的目录作为项目目录。
- 读取 `./dev-manager-ai-space/rules/directory-structure.md`，按照文件中的目录结构创建目录和文件，注意：只创建确定的目录和文件，不要创建多余的目录和文件。
- 将 `./templates/` 下的文件复制到项目的 `templates/` 下。
- 将 `./templates/项目README模板.md` 复制到项目根目录并改名为 `README.md`，作为项目根 README；初始化时保留模板中的占位文本（如"这里是项目概述"），由用户后续填写。
- 询问用户前端项目的地址 `$frontendProjPath`。
- 询问用户后端项目的地址 `$backendProjPath`。
- 创建项目管理目录下的 `setting.local.json` 文件，结构参见 `templates/开发管理设置模板.md` 第 1 节，将 `projectPath` 占位符分别替换为 `$frontendProjPath` 与 `$backendProjPath`。
- 在项目管理目录下创建需求分析师的 AI Agent，agent 的配置查看[需求分析师配置](./需求分析师Agent.md)。
- 在项目管理目录下创建架构师的 AI Agent，agent 的配置查看[架构师配置](./架构师Agent.md)。
- 在项目管理目录下创建 AI rules，按照 AI 工具的规则，复制 [rules 文件夹](./dev-manager-ai-space/rules/) 下的内容。
- 在项目管理目录下创建 AI skills，按照 AI 工具的规则，复制 [skills 文件夹](./dev-manager-ai-space/skills/) 下的内容。

- 在前端项目空间下创建前端开发者 AI Agent，agent 的配置查看[前端开发者Agent](./前端开发者Agent.md)。
- 在前端项目空间下创建 AI rules，按照 AI 工具的规则，复制 [rules 文件夹](./frontend-ai-space/rules/) 下的内容。
- 在前端项目空间下创建 AI skills，按照 AI 工具的规则，复制 [skills 文件夹](./frontend-ai-space/skills/) 下的内容；再将 [公共 skills 文件夹](./common-ai-space/skills/) 下的 `task-driven-dev` 复制到前端项目空间，复制时将文件中的 `{{taskFile}}` 占位符全部替换为 `frontend.md`。
- 在前端项目根目录下创建 `setting.local.json`，记录全栈项目管理目录的根路径（即当前项目目录的绝对路径），结构参见 `templates/开发管理设置模板.md` 第 2 节。

- 在后端项目空间下创建后端开发者 AI Agent，agent 的配置查看[后端开发者Agent](./后端开发者Agent.md)。
- 在后端项目空间下创建 AI rules，按照 AI 工具的规则，复制 [rules 文件夹](./backend-ai-space/rules/) 下的内容。
- 在后端项目空间下创建 AI skills，按照 AI 工具的规则，复制 [skills 文件夹](./backend-ai-space/skills/) 下的内容；再将 [公共 skills 文件夹](./common-ai-space/skills/) 下的 `task-driven-dev` 复制到后端项目空间，复制时将文件中的 `{{taskFile}}` 占位符全部替换为 `backend.md`。
- 在后端项目根目录下创建 `setting.local.json`，记录全栈项目管理目录的根路径（即当前项目目录的绝对路径），结构参见 `templates/开发管理设置模板.md` 第 2 节。
