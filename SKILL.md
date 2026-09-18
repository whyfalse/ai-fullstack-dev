---
name: ai-fullstack-dev
description: 创建一个AI全栈开发项目
version: 2.1.0
---

请严格按照下面的流程进行创建：

> 说明：本仓库中的 `dev-manager-ai-space/`、`frontend-ai-space/`、`backend-ai-space/`、`common-ai-space/` 四个目录以及 `SKILL.md` 是**模板源文件**，仅用于初始化时读取，不需要复制到新项目。新项目只需复制 `templates/` 目录，以及 `dev-manager-ai-space/rules/`、各 `*-ai-space/skills/` 与 `common-ai-space/skills/task-driven-dev` 下的内容到对应位置；`task-driven-dev` 来自 `common-ai-space/`，初始化时复制到前端与后端两个空间，并按下方步骤替换占位符。不向代码仓安装任何 rules。
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
- 否则按步骤 1-9 执行创建流程。

## 1. 确认项目目录与代码仓库路径

- 与用户确认是否使用当前目录作为项目管理目录（dev-manager space）。
- 通过一次交互式提问（多个问题、支持自由输入）同时询问用户的前端项目路径 `$frontendProjPath` 与后端项目路径 `$backendProjPath`。
- 拿到两个路径后，**立即验证其是否存在**；若不存在则提示用户重新确认，避免后续写入 `setting.local.json` 后 Agent 读取失败。

## 2. 确认用户使用的 AI 工具

- 通过交互式提问（支持自由输入）询问用户在新项目中使用的 AI 编码工具。记为 `$aiTool`。
- 该选择决定后续 Agent 文件的生成格式、rules 与 skills 的存放位置，以及 Agent 如何引用 skills。
- 同一次交互式提问中追加确认：`$aiTool` 是否具备**子代理派发能力**（能否在主对话中以子代理方式调用已安装的 Agent 定义并获取其结果，如 Claude Code 的 sub-agent 机制）。记为 `$subagentCapable`：已知具备的工具（如 Claude Code）默认为 true 并向用户说明；无法判断时询问用户；不具备则记为 false。
- `$subagentCapable` 为 true 时，步骤 5 会额外生成项目经理 Agent；为 false 时仅跳过该项。开发者 Agent 与跨仓权限配置不依赖该开关（手动模式同样需要），各 Agent 的手动调用模式不受影响。
- 请用户提供所用工具的 Agent / rules / skills 配置规范（目录约定、文件格式、引用方式），按该规范生成；如用户无法提供规范但清楚工具名称，按该工具公开的通用约定类比生成；如用户均不清楚，生成一份通用的 Agent 定义文件（保留 `name`、`description`、`required-skills` 与系统提示正文），并明确告知用户需按所用工具自行适配。

## 3. 创建项目管理目录骨架

- 读取 `./dev-manager-ai-space/rules/directory-structure.md`，按照其中的目录结构创建目录和文件。注意：只创建确定的目录和文件，不要创建多余的目录和文件；**其中的 `setting.local.json` 跳过，由步骤 4 写入**；**`framework.json` 跳过，由步骤 8 写入**；**`tester/config.md` 跳过，由架构师首次架构设计时产出（参照项目 `templates/tester-config-template.md`）**；**`tester/config.local.md` 跳过，由开发者个人按需创建**；**`dev-tasks/pipeline-log.md` 跳过，由项目经理 Agent 首次记录时按需创建**；`tester/reports/` 创建空目录。
- 将 `./templates/` 下的文件复制到项目的 `templates/` 下。
- 将 `./init-templates/项目README模板.md` 复制到项目根目录并改名为 `README.md`；初始化时保留模板中的占位文本（如"这里是项目概述"），由用户后续填写。

## 4. 写入项目管理目录的 setting.local.json

- 在项目管理目录下创建 `setting.local.json`，结构参见 `./init-templates/开发管理设置模板.md` 第 1 节，将 `projectPath` 占位符分别替换为 `$frontendProjPath` 与 `$backendProjPath`。

## 5. 创建项目管理目录的 Agent / rules / skills

- **Agent**：读取 `./dev-manager-ai-space/agents/` 下的 `需求分析师Agent.md`、`架构师Agent.md`、`测试者Agent.md` 与 `项目经理Agent.md`（最后一项仅当 `$subagentCapable` 为 true），以及 `./common-ai-space/agents/开发者Agent.md`，依据步骤 2 确认的 `$aiTool`，将该 Agent 定义（`name`、`description`、`required-skills`、正文系统提示）转换为对应 AI 工具的 Agent 配置格式后，写入项目管理目录中该工具约定的位置。生成时需遵循以下原则：
  - 保留 `name`、`description` 与正文系统提示的原文，不篡改 Agent 的行为描述。
  - 将 `required-skills` 中列出的 skill 名称映射为目标工具的 skill 引用方式；这些 skill 必须能在下一步复制进来的 skills 目录中找到对应文件夹。
  - 目标工具专有的字段（如可调用工具集、模型、颜色、记忆范围等）按该工具的默认规范设置，不在源定义中固化。
  - 生成的 Agent 文件写入项目管理目录中 `$aiTool` 约定的 agent 存放位置；若该工具以规则/提示文件等价承载 Agent，则按其等价格式生成。
- **开发者 Agent（统一安装于项目管理空间）**：读取 `./common-ai-space/agents/开发者Agent.md`，为前端、后端两侧各生成一份目标工具格式的 Agent 文件，写入**项目管理目录**中 `$aiTool` 约定的 agent 存放位置。**无条件生成**（手动模式同样在项目管理空间调用开发者，不依赖 `$subagentCapable`）。生成规则：
  - `name` 按侧定为 `frontend-developer` / `backend-developer`；
  - 正文中的 `{{spaceType}}` 按侧替换为 `前端` / `后端`；
  - `required-skills` 保持置空：`task-driven-dev` / `dev-thinking` 及各侧专有技能安装在代码仓（前后端为同名冲突的两版），技能经 Agent 正文的"开工前必读"清单以 `Read` 方式加载，转换时不得为其生成技能引用；
  - 生成时移除正文开头引用块形式的模板说明（`> 本文件为模板源…` 整段）——该段面向模板源维护者，不属于运行时系统提示，且其中提及 `{{spaceType}}` 替换规则，保留会在生成副本中产生乱码文本；
  - 其余转换原则与上面 Agent 生成一致。
- **工具权限设置**：在项目管理目录中按 `$aiTool` 的权限机制配置（开发者 Agent 统一安装于项目管理空间后，手动模式同样需要跨仓读写，故无条件配置）：
  - 将 `$frontendProjPath` 与 `$backendProjPath` 的绝对路径加入**附加目录授权**——仅授予文件读写权、不加载附加目录的任何配置（如 Claude Code 的 `permissions.additionalDirectories`）；
  - 配置**危险命令拒绝规则**：git push、文件/目录删除类、数据库 DDL 执行类命令一律 deny；
  - 所用工具无目录级权限机制时，改为在完成提示中说明需用户自行授予跨目录读写权限，并警示不得以跳过权限检查的模式运行。
- **rules**：在项目管理目录下，按 `$aiTool` 的规则目录约定，复制 `./dev-manager-ai-space/rules/` 下的内容。
- **skills**：在项目管理目录下，按 `$aiTool` 的 skills 目录约定，复制 `./dev-manager-ai-space/skills/` 下的内容。

## 6. 创建前端与后端项目空间（两空间互不依赖，可并行执行）

对前端项目空间（`$frontendProjPath`）和后端项目空间（`$backendProjPath`）分别执行以下步骤，两空间目标目录不同、可并行。**注意：代码仓不安装任何 Agent（开发者 Agent 已统一安装于项目管理目录，见步骤 5），也不创建仓侧 `setting.local.json`，也不安装任何 rules（仓内仅保留用户自己的约束文件）；本步骤安装的 skills 是流程纪律的载体，供开发者 Agent 以 `Read` 方式加载。**

1. **创建 AI skills**：按 `$aiTool` 的 skills 目录约定，复制对应空间 `*-ai-space/skills/` 下的内容到对应代码项目；再将 `./common-ai-space/skills/` 下的 `task-driven-dev` 复制进来，复制时将文件中的 `{{taskFile}}` 占位符全部替换为对应空间的任务文件名（前端替换为 `frontend.md`，后端替换为 `backend.md`）。

## 7. 记录前后端技术栈

- 优先从前后端项目空间自动读取技术栈（如 `package.json`、`pom.xml`/`build.gradle`、`go.mod`、`requirements.txt`/`pyproject.toml` 等清单文件），无法识别时再请用户直接输入。
- 将识别/确认后的前后端技术栈记录到项目管理目录下的 `architecture/README.md` 中。

## 8. 写入框架元信息

- 在项目管理目录根下创建 `framework.json`，字段定义、结构示例与维护规则参见 `./init-templates/框架元信息模板.md`。
- 写入时：`version` 读取本仓库根目录 `SKILL.md` frontmatter 中的 `version` 字段，`initializedAt` 使用初始化日期。

## 9. 完成提示

- 初始化全部完成后，提示用户：下一步可调用**需求分析师 Agent** 启动需求收集流程。
- 提示用户：开发者 Agent（`frontend-developer` / `backend-developer`）已安装在项目管理空间，手动开发时在当前空间直接调用即可，无需切换到前后端代码仓开启新对话。
- 若 `$subagentCapable` 为 true，补充提示：也可调用**项目经理 Agent** 发起自动化全流程（需求分析 → 架构设计 → 前后端开发 → 测试，一次对话完成），各契约门仍会请求人工批复；手动调用各 Agent 的用法完全不变。
- 提示用户：`tester/config.md`（测试账号/工具/环境）将在首次调用架构师进行架构设计时由架构师生成骨架，届时按提示补充账号细节；个人差异配置写在 `tester/config.local.md`（不入库）。
