---
name: ai-fullstack-dev
description: 创建一个AI全栈开发项目
version: 2.4.0
---

按下面的流程创建：

> 说明：本仓库中的 `dev-manager-ai-space/`、`common-ai-space/` 两个目录以及 `SKILL.md` 是**模板源文件**，仅用于初始化时读取，不需要复制到新项目。新项目只需复制 `templates/` 目录与 `dev-manager-ai-space/rules/`、`dev-manager-ai-space/skills/` 下的内容到项目管理目录；代码仓不安装任何 rules、skills、Agent 或配置文件，保持零框架文件。
>
> 各 `*-ai-space/agents/` 与 `common-ai-space/agents/` 下的 `*Agent.md` 是**与 AI 工具无关的 Agent 定义**（仅含 `name`、`description`、`required-skills` 与系统提示正文），初始化时不直接复制，而是按步骤 2 确认的 AI 工具动态生成对应格式的 Agent 文件。所有 Agent（含开发者）统一生成到新项目的项目管理目录，代码仓不安装任何 Agent。
>
> `init-templates/` 存放**仅初始化/升级流程使用**的模板（开发管理设置、框架元信息、项目 README），不会复制到新项目的 `templates/` 目录；`templates/` 下只存放工作流项目运行过程中使用的模板文件，初始化时整体复制到新项目。
>
> `升级流程.md` 是**升级已有项目的流程定义**，仅当走升级路线时读取执行。

## 0. 路由：创建流程还是升级流程

- 若满足以下任一条件，转入 `./升级流程.md` 执行升级，**不再走下面的创建流程**：
  - 用户明确表达升级意图（如"升级该项目"、"同步框架最新版本"）；
  - 目标目录已存在本框架初始化的项目（存在 `framework.json`，或存在 `requirements/`、`architecture/`、`dev-tasks/`、`tester/` 等骨架目录）。
- 否则按步骤 1-8 执行创建流程。

## 1. 确认项目目录与代码模块

- 与用户确认是否使用当前目录作为项目管理目录（dev-manager space）。
- 通过交互式提问收集项目的全部**代码模块**，每个模块需提供：模块 ID（`id`，全项目唯一、小写英文、不含空格）、名称（`name`，可用中文）、类型（`type`，`frontend` / `backend`）、代码仓路径（`path`）、依赖（`dependsOn`，可空，仅信息标注）。经典双模块项目即 `frontend`（前端）+ `backend`（后端）两个模块；至少一个模块。
- 收集完成后立即校验：
  - **ID 唯一**：各模块 `id` 不得重复；
  - **路径存在**：各模块 `path` 必须存在，不存在则提示用户重新确认，避免写入 `setting.local.json` 后 Agent 读取失败；
  - **依赖不成环**：`dependsOn` 声明的依赖不得构成环（如 A 依赖 B、B 依赖 A）。
- 校验不通过则让用户修正后重试；通过后将模块清单记为 `$modules`（数组，元素含 `id`/`name`/`type`/`path`/`dependsOn`），供后续步骤使用。

## 2. 确认用户使用的 AI 工具

- 通过交互式提问（支持自由输入）询问用户在新项目中使用的 AI 编码工具。记为 `$aiTool`。
- 该选择决定后续 Agent 文件的生成格式、rules 与 skills 的存放位置，以及 Agent 如何引用 skills。
- 同一次交互式提问中追加确认：`$aiTool` 是否具备**子代理派发能力**（能否在主对话内以子代理机制直接调用已安装的 Agent 定义并获取其结果与完成通知——前台派发阻塞等待返回、后台派发完成即自动通知，无需另起独立 CLI 进程，如 Claude Code 的 Agent 工具）。记为 `$subagentCapable`：已知具备的工具（如 Claude Code）默认为 true 并向用户说明；无法判断时询问用户；不具备则记为 false。
- `$subagentCapable` 为 true 时，步骤 5 会额外生成项目经理 Agent；为 false 时仅跳过该项。开发者 Agent 与跨仓权限配置不依赖该开关（手动模式同样需要），各 Agent 的手动调用模式不受影响。
- 请用户提供所用工具的 Agent / rules / skills 配置规范（目录约定、文件格式、引用方式），按该规范生成；如用户无法提供规范但清楚工具名称，按该工具公开的通用约定类比生成；如用户均不清楚，生成一份通用的 Agent 定义文件（保留 `name`、`description`、`required-skills` 与系统提示正文），并明确告知用户需按所用工具自行适配。

## 3. 创建项目管理目录骨架

- 读取 `./dev-manager-ai-space/rules/directory-structure.md`，按照其中的目录结构创建目录和文件。注意：只创建确定的目录和文件，不要创建多余的目录和文件；**其中的 `setting.local.json` 跳过，由步骤 4 写入**；**`modules.json` 跳过，由步骤 4 写入**；**`framework.json` 跳过，由步骤 7 写入**；**`tester/config.md` 跳过，由架构师首次架构设计时产出（参照项目 `templates/tester-config-template.md`）**；**`tester/config.local.md` 跳过，由开发者个人按需创建**；**`dev-tasks/pipeline-log.md` 跳过，由项目经理 Agent 首次记录时按需创建**；`tester/reports/` 创建空目录。
- 将 `./templates/` 下的文件复制到项目的 `templates/` 下。
- 将 `./init-templates/项目README模板.md` 复制到项目根目录并改名为 `README.md`；初始化时保留模板中的占位文本（如"这里是项目概述"），由用户后续填写。

## 4. 写入模块注册表与项目设置

- 在项目管理目录根下创建 `modules.json`，结构参见 `./init-templates/模块注册表模板.md`：将 `$modules` 中各模块的 `id`/`name`/`type`/`owner`/`dependsOn` 写入 `modules` 数组（`owner` 第 1 步默认 `null`）。
- 在项目管理目录根下创建 `setting.local.json`，结构参见 `./init-templates/开发管理设置模板.md` 第 1 节：将各模块的 `path` 写入 `modules.<id>.projectPath`。

## 5. 创建项目管理目录的 Agent / rules / skills

- **Agent**：读取 `./dev-manager-ai-space/agents/` 下的 `需求分析师Agent.md`、`架构师Agent.md`、`测试者Agent.md` 与 `项目经理Agent.md`（最后一项仅当 `$subagentCapable` 为 true），以及 `./common-ai-space/agents/开发者Agent.md`，依据步骤 2 确认的 `$aiTool`，将该 Agent 定义（`name`、`description`、`required-skills`、正文系统提示）转换为对应 AI 工具的 Agent 配置格式后，写入项目管理目录中该工具约定的位置。生成时需遵循以下原则：
  - 保留 `name`、`description` 与正文系统提示的原文，不篡改 Agent 的行为描述。
  - 将 `required-skills` 中列出的 skill 名称映射为目标工具的 skill 引用方式；这些 skill 必须能在下一步复制进来的 skills 目录中找到对应文件夹。
  - 目标工具专有的字段（如可调用工具集、模型、颜色、记忆范围等）按该工具的默认规范设置，不在源定义中固化。
  - 生成的 Agent 文件写入项目管理目录中 `$aiTool` 约定的 agent 存放位置；若该工具以规则/提示文件等价承载 Agent，则按其等价格式生成。
- **开发者 Agent（统一安装于项目管理空间）**：读取 `./common-ai-space/agents/开发者Agent.md`，为 `$modules` 中的每个代码模块各生成一份目标工具格式的 Agent 文件，写入**项目管理目录**中 `$aiTool` 约定的 agent 存放位置。经典双模块项目即生成 `frontend-developer` / `backend-developer` 两份。**无条件生成**（手动模式同样在项目管理空间调用开发者，不依赖 `$subagentCapable`）。生成规则：
  - `name` 按模块 ID 定为 `<moduleId>-developer`（如模块 ID 为 `frontend` 即 `frontend-developer`）；
  - 正文中的 `{{moduleId}}` 按模块替换为对应模块 ID（如 `frontend` / `backend`）；
  - `required-skills` 保持置空：`task-driven-dev`（单副本）与 `dev-thinking`（通用原则 `SKILL.md` + 按模块类型取用的 `frontend.md` / `backend.md` 端文件）均安装在项目管理空间，技能经 Agent 正文的"开工前必读"清单以 `Read` 方式加载，转换时不得为其生成技能引用；
  - 其余转换原则与上面 Agent 生成一致。
- **工具权限设置**：在项目管理目录中按 `$aiTool` 的权限机制配置（开发者 Agent 统一安装于项目管理空间后，手动模式同样需要跨仓读写，故无条件配置）：
  - 将 `$modules` 中每个模块的 `path` 绝对路径加入**附加目录授权**——仅授予文件读写权、不加载附加目录的任何配置（如 Claude Code 的 `permissions.additionalDirectories`）；
  - 配置**危险命令拒绝规则**：git push、文件/目录删除类、数据库 DDL 执行类命令一律 deny；
  - 所用工具无目录级权限机制时，改为在完成提示中说明需用户自行授予跨目录读写权限，并警示不得以跳过权限检查的模式运行。
- **rules**：在项目管理目录下，按 `$aiTool` 的规则目录约定，复制 `./dev-manager-ai-space/rules/` 下的内容。
- **skills**：在项目管理目录下，按 `$aiTool` 的 skills 目录约定，复制 `./dev-manager-ai-space/skills/` 下的内容。

## 6. 记录各模块技术栈

- 对 `$modules` 中的每个模块，优先从该模块代码仓自动读取技术栈（如 `package.json`、`pom.xml`/`build.gradle`、`go.mod`、`requirements.txt`/`pyproject.toml` 等清单文件），无法识别时再请用户直接输入。
- 将识别/确认后的各模块技术栈按模块分节记录到项目管理目录下的 `architecture/README.md` 中。

## 7. 写入框架元信息

- 在项目管理目录根下创建 `framework.json`，字段定义、结构示例与维护规则参见 `./init-templates/框架元信息模板.md`。
- 写入时：`version` 读取本仓库根目录 `SKILL.md` frontmatter 中的 `version` 字段，`initializedAt` 使用初始化日期。

## 8. 完成提示

- 初始化全部完成后，提示用户：下一步可调用**需求分析师 Agent** 启动需求收集流程。
- 提示用户：各模块的开发者 Agent（`<moduleId>-developer`）已安装在项目管理空间，手动开发时在当前空间直接调用即可，无需切换到对应代码仓开启新对话。
- 若 `$subagentCapable` 为 true，补充提示：也可调用**项目经理 Agent** 发起自动化全流程（需求分析 → 架构设计 → 各模块开发 → 测试，一次对话完成），各契约门仍会请求人工批复；手动调用各 Agent 的用法完全不变。
- 提示用户：`tester/config.md`（测试账号/工具/环境）将在首次调用架构师进行架构设计时由架构师生成骨架，届时按提示补充账号细节；个人差异配置写在 `tester/config.local.md`（不入库）。
