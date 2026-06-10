---
name: 全栈开发项目创建
description: 创建一个AI全栈开发项目
---

请严格按照下面的流程进行创建：

- 让用户确认是否要在当前的工作目录下创建项目
- 读取`./backend-ai-space/rules/directory.md`，按照文件中的目录结构创建目录和文件，注意：只创建确定的目录和文件，不要创建多余的目录和文件。
- 将`./appendix/`下的文件复制到项目的`appendix/`下。
- 询问用户前端项目的地址`$frontendProjPath`。
- 询问用户后端项目的地址`$backendProjPath`。
- 创建`setting.local.json`文件，内容如下：
    ``` json
    {
        "frontend": {
            "projectPath": $frontendProjPath
        },
        "backend": {
            "projectPath": $backendProjPath
        }
    }
    ```
- 在项目空间下创建需求分析师的AI Agent，agent的配置查看[需求分析师配置](./需求分析师Agent.md)。
- 在项目空间下创建架构师的AI Agent，agent的配置查看[架构师配置](./架构师Agent.md)。
- 在项目空间下创建AI rules，直接复制[rules](./rules/)下的内容。
- 在项目空间下创建AI skills，直接复制[skills](./skills/)下的内容。
- 在前端项目空间下创建前端开发者AI Agent，agent的配置查看[前端开发者Agent](./前端开发者Agent.md)。
- 在前端项目空间下创建AI rules，直接复制[rules](./rules/)下的内容。
- 在前端项目空间下创建AI skills，直接复制[skills](./skills/)下的内容。
- 在后端项目空间下创建后端开发者AI Agent，agent的配置查看[后端开发者Agent](./后端开发者Agent.md)。
- 在后端项目空间下创建AI rules，直接复制[rules](./rules/)下的内容。
- 在后端项目空间下创建AI skills，直接复制[skills](./skills/)下的内容。
