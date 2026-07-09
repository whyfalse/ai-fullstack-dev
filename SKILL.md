---
name: ai-fullstack-dev
description: 创建一个AI全栈开发项目
---

请严格按照下面的流程进行创建：

> 说明：本仓库中的 `dev-manager-ai-space/`、`frontend-ai-space/`、`backend-ai-space/`、`common-ai-space/` 四个目录以及 `需求分析师Agent.md`、`架构师Agent.md`、`前端开发者Agent.md`、`后端开发者Agent.md`、`SKILL.md` 是**模板源文件**，仅用于初始化时读取，不需要复制到新项目。新项目只需复制 `templates/` 目录，以及各 `*-ai-space/rules/` 和 `*-ai-space/skills/` 下的内容到对应位置。`common-ai-space/skills/` 下为前后端共享的技能模板，初始化时需复制到前端与后端两个空间，并按下方步骤替换占位符。

## 1. 确认项目目录与代码仓库路径

- 与用户确认是否使用当前目录作为项目管理目录（dev-manager space）。
- 通过一次 `AskUserQuestion` 同时询问用户的前端项目路径 `$frontendProjPath` 与后端项目路径 `$backendProjPath`（用户可通过 "Other" 自由输入完整路径）。
- 拿到两个路径后，**立即验证其是否存在**；若不存在则提示用户重新确认，避免后续写入 `setting.local.json` 后 Agent 读取失败。

## 2. 创建项目管理目录骨架

- 读取 `./dev-manager-ai-space/rules/directory-structure.md`，按照其中的目录结构创建目录和文件。注意：只创建确定的目录和文件，不要创建多余的目录和文件；**其中的 `setting.local.json` 跳过，由步骤 3 写入**。
- 将 `./templates/` 下的文件复制到项目的 `templates/` 下。
- 将 `./templates/项目README模板.md`（与上一步同源）复制到项目根目录并改名为 `README.md`；初始化时保留模板中的占位文本（如"这里是项目概述"），由用户后续填写。

## 3. 写入项目管理目录的 setting.local.json

- 在项目管理目录下创建 `setting.local.json`，结构参见 `templates/开发管理设置模板.md` 第 1 节，将 `projectPath` 占位符分别替换为 `$frontendProjPath` 与 `$backendProjPath`。

## 4. 创建项目管理目录的 Agent / rules / skills

- 在项目管理目录下创建需求分析师的 AI Agent，agent 的配置查看[需求分析师配置](./需求分析师Agent.md)。
- 在项目管理目录下创建架构师的 AI Agent，agent 的配置查看[架构师配置](./架构师Agent.md)。
- 在项目管理目录下创建 AI rules，按照 AI 工具的规则，复制 [rules 文件夹](./dev-manager-ai-space/rules/) 下的内容。
- 在项目管理目录下创建 AI skills，按照 AI 工具的规则，复制 [skills 文件夹](./dev-manager-ai-space/skills/) 下的内容。

## 5. 创建前端与后端项目空间（两空间互不依赖，可并行执行）

对前端项目空间（`$frontendProjPath`）和后端项目空间（`$backendProjPath`）分别执行以下步骤，两空间目标目录不同、可并行：

1. 创建对应的开发者 AI Agent：前端查看[前端开发者Agent](./前端开发者Agent.md)，后端查看[后端开发者Agent](./后端开发者Agent.md)。
2. 创建 AI rules：复制对应空间 `*-ai-space/rules/` 下的内容（前端用 `frontend-ai-space/rules/`，后端用 `backend-ai-space/rules/`）。
3. 创建 AI skills：复制对应空间 `*-ai-space/skills/` 下的内容；再将 [公共 skills 文件夹](./common-ai-space/skills/) 下的 `task-driven-dev` 复制进来，复制时将文件中的 `{{taskFile}}` 占位符全部替换为对应空间的任务文件名（前端替换为 `frontend.md`，后端替换为 `backend.md`）。
4. 在对应代码项目根目录下创建 `setting.local.json`，记录全栈项目管理目录的根路径（即当前项目目录的绝对路径），结构参见 `templates/开发管理设置模板.md` 第 2 节。

## 6. 记录前后端技术栈

- 优先从前后端项目空间自动读取技术栈（如 `package.json`、`pom.xml`/`build.gradle`、`go.mod`、`requirements.txt`/`pyproject.toml` 等清单文件），无法识别时再请用户直接输入。
- 将识别/确认后的前后端技术栈记录到项目管理目录下的 `architecture/README.md` 中。

## 7. 完成提示

- 初始化全部完成后，提示用户：下一步可调用**需求分析师 Agent** 启动需求收集流程。
