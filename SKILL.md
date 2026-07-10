---
name: ai-fullstack-dev
description: 创建一个AI全栈开发项目
---

请严格按照下面的流程进行创建：

> 说明：本仓库中的 `dev-manager-ai-space/`、`frontend-ai-space/`、`backend-ai-space/`、`common-ai-space/` 四个目录以及 `需求分析师Agent.md`、`架构师Agent.md`、`前端开发者Agent.md`、`后端开发者Agent.md`、`SKILL.md` 是**模板源文件**，仅用于初始化时读取，不需要复制到新项目。新项目只需复制 `templates/` 目录，以及 `dev-manager-ai-space/rules/` 和各 `*-ai-space/skills/` 下的内容到对应位置；前后端的 rules 与 `common-ai-space/skills/task-driven-dev` 均来自 `common-ai-space/`，初始化时复制到前端与后端两个空间，并按下方步骤替换占位符。
>
> 四个 `*Agent.md` 是**与 AI 工具无关的 Agent 定义**（仅含 `name`、`description`、`required-skills` 与系统提示正文），初始化时不直接复制，而是按步骤 2 确认的 AI 工具动态生成对应格式的 Agent 文件。

## 1. 确认项目目录与代码仓库路径

- 与用户确认是否使用当前目录作为项目管理目录（dev-manager space）。
- 通过一次 `AskUserQuestion` 同时询问用户的前端项目路径 `$frontendProjPath` 与后端项目路径 `$backendProjPath`（用户可通过 "Other" 自由输入完整路径）。
- 拿到两个路径后，**立即验证其是否存在**；若不存在则提示用户重新确认，避免后续写入 `setting.local.json` 后 Agent 读取失败。

## 2. 确认用户使用的 AI 工具

- 通过 `AskUserQuestion` 询问用户在新项目中使用的 AI 工具（如 Claude Code、Cursor、Windsurf、Cline 等；用户可通过 "Other" 自由输入）。记为 `$aiTool`。
- 该选择决定后续 Agent 文件的生成格式、rules 与 skills 的存放位置，以及 Agent 如何引用 skills。
- 若用户选择的工具不在已知列表中，按用户提供的该工具 Agent 配置规范生成；如用户不清楚，默认按 Claude Code 规范生成并明确告知用户。

## 3. 创建项目管理目录骨架

- 读取 `./dev-manager-ai-space/rules/directory-structure.md`，按照其中的目录结构创建目录和文件。注意：只创建确定的目录和文件，不要创建多余的目录和文件；**其中的 `setting.local.json` 跳过，由步骤 4 写入**。
- 将 `./templates/` 下的文件复制到项目的 `templates/` 下。
- 将 `./templates/项目README模板.md`（与上一步同源）复制到项目根目录并改名为 `README.md`；初始化时保留模板中的占位文本（如"这里是项目概述"），由用户后续填写。

## 4. 写入项目管理目录的 setting.local.json

- 在项目管理目录下创建 `setting.local.json`，结构参见 `templates/开发管理设置模板.md` 第 1 节，将 `projectPath` 占位符分别替换为 `$frontendProjPath` 与 `$backendProjPath`。

## 5. 创建项目管理目录的 Agent / rules / skills

- **Agent**：读取 `./需求分析师Agent.md` 与 `./架构师Agent.md`，依据步骤 2 确认的 `$aiTool`，将该 Agent 定义（`name`、`description`、`required-skills`、正文系统提示）转换为对应 AI 工具的 Agent 配置格式后，写入项目管理目录中该工具约定的位置。生成时需遵循以下原则：
  - 保留 `name`、`description` 与正文系统提示的原文，不篡改 Agent 的行为描述。
  - 将 `required-skills` 中列出的 skill 名称映射为目标工具的 skill 引用方式；这些 skill 必须能在下一步复制进来的 skills 目录中找到对应文件夹。
  - 目标工具专有的字段（如可调用工具集、模型、颜色、记忆范围等）按该工具的默认规范设置，不在源定义中固化。
  - 已知工具的生成方式（若用户使用其他工具，按其规范类比生成）：
    - **Claude Code**：在项目管理目录的 `.claude/agents/` 下生成 `<name>.md`，frontmatter 包含 `name`、`description`、`tools`（按 Agent 角色配置合理默认工具集）、`model: inherit`、`color`、`memory: project`、`skills`（取自 `required-skills`），正文为系统提示原文。
    - **Cursor**：在 `.cursor/rules/` 下按 Cursor 的 agent/rule 格式生成对应文件，将 `required-skills` 在正文中以引用方式列出。
    - **Windsurf / Cline 等**：按各自约定的 agent 目录与格式生成。
- **rules**：在项目管理目录下，按 `$aiTool` 的规则目录约定，复制 `./dev-manager-ai-space/rules/` 下的内容。
- **skills**：在项目管理目录下，按 `$aiTool` 的 skills 目录约定，复制 `./dev-manager-ai-space/skills/` 下的内容。

## 6. 创建前端与后端项目空间（两空间互不依赖，可并行执行）

对前端项目空间（`$frontendProjPath`）和后端项目空间（`$backendProjPath`）分别执行以下步骤，两空间目标目录不同、可并行：

1. **创建对应的开发者 AI Agent**：分别读取 `./前端开发者Agent.md` 与 `./后端开发者Agent.md`，依据步骤 2 确认的 `$aiTool`，按与步骤 5 相同的转换规则生成目标工具格式的 Agent 文件，写入对应代码项目约定的 agent 目录。
2. **创建 AI rules**：按 `$aiTool` 的规则目录约定，复制 `./common-ai-space/rules/` 下的内容到对应代码项目。
3. **创建 AI skills**：按 `$aiTool` 的 skills 目录约定，复制对应空间 `*-ai-space/skills/` 下的内容到对应代码项目；再将 `./common-ai-space/skills/` 下的 `task-driven-dev` 复制进来，复制时将文件中的 `{{taskFile}}` 占位符全部替换为对应空间的任务文件名（前端替换为 `frontend.md`，后端替换为 `backend.md`）。
4. 在对应代码项目根目录下创建 `setting.local.json`，记录全栈项目管理目录的根路径（即当前项目目录的绝对路径），结构参见 `templates/开发管理设置模板.md` 第 2 节。

## 7. 记录前后端技术栈

- 优先从前后端项目空间自动读取技术栈（如 `package.json`、`pom.xml`/`build.gradle`、`go.mod`、`requirements.txt`/`pyproject.toml` 等清单文件），无法识别时再请用户直接输入。
- 将识别/确认后的前后端技术栈记录到项目管理目录下的 `architecture/README.md` 中。

## 8. 完成提示

- 初始化全部完成后，提示用户：下一步可调用**需求分析师 Agent** 启动需求收集流程。
