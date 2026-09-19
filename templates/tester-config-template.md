# 测试配置模板（config.md / config.local.md）

> 本模板定义 `/tester/config.md`（全局配置，团队共享、入库）的结构，并说明 `/tester/config.local.md`（私人配置，个人覆盖、不入库）的覆盖方式。架构师首次架构设计时按本模板初始化 `config.md` 骨架（工具选型参照 `/architecture/README.md` 的技术栈），测试者运行时读取 `config.md` 并以 `config.local.md` 覆盖合并。

## config.md 结构

按以下三节组织。

### 测试账号

seed 测试账号，供测试者登录与功能验证使用。团队统一维护，确保结果可复现。

| 账号 | 密码 | 角色 | 用途 | 初始化来源 |
| ---- | ---- | ---- | ---- | ---------- |
| tester01 | Pass123 | 普通用户 | 常规功能验证 | seed 脚本 `scripts/seed.ts` |
| admin01 | Admin123 | 管理员 | 鉴权与权限用例 | seed 脚本 `scripts/seed.ts` |

> 敏感密码不入明文：可在此只记账号标识与引用，密码从环境变量读取（如 `${TEST_USER_PWD}`）。

### 测试工具

团队统一选型的测试工具与执行命令。单元测试命令按模块分列（模块清单见 `modules.json`）；E2E 框架一经选定不可个人替换（影响 `test.md` 用例编写）。

**单元测试（按模块）**

| 模块 | 单元测试命令 |
| ---- | ----------- |
| backend | `npm run test`（如 `pytest`） |
| frontend | `npm run test` |

- E2E 框架：`<框架>`（如 Playwright / Cypress）
- E2E 执行命令：`<命令>`（如 `npx playwright test` / `npx cypress run`；留空或为占位符则 E2E 用例回退人工验证）
- E2E 宿主模块：`<moduleId>`（E2E 脚本所在的前端模块仓，执行 E2E 时在该仓路径下运行）
- API 测试：`<方式>`（如 curl / Postman / IDEA HTTP Client）

### 测试环境

标准本地环境地址，团队约定的默认环境，按模块分列；个人环境地址可在 `config.local.md` 覆盖。

**服务地址（按模块）**

| 模块 | 服务地址 |
| ---- | -------- |
| backend | `http://localhost:8080` |
| frontend | `http://localhost:3000` |

- 测试数据库：`<连接信息或引用>`

## config.local.md 结构

`config.local.md` 为个人覆盖配置，**结构同 config.md 三节**，只填与全局不同的项。读取时按"私人覆盖全局，未覆盖项回退全局默认"合并；不写此文件则直接用全局默认。

```markdown
# 测试配置（个人覆盖）

### 测试工具

- backend 单元测试：`./node_modules/.bin/jest`（Windows 路径差异覆盖）
- E2E 执行命令：`npx playwright test --project=chromium`（个人只跑 Chromium，框架选型不变）

### 测试环境

- backend 服务：`http://localhost:8081`（个人起了不同端口）

### 测试账号

| 账号 | 密码 | 角色 | 用途 |
| ---- | ---- | ---- | ---- |
| myuser | MyPass123 | 普通用户 | 个人调试账号 |
```

## 约定

- `config.md` 为团队基座、入库；`config.local.md` 为个人覆盖、不入库（见 `.gitignore`）。
- 可个人覆盖：工具命令/路径、环境地址、个人调试账号。
- 不可个人覆盖（影响复现与用例）：seed 账号集、E2E 框架选型。
- 测试报告元信息须记录本次实际使用的账号集与环境（标注哪些来自个人覆盖），保证可复现。
- `config.md` 由架构师首次架构设计时产出骨架；后续工具/环境重大变更时由架构师更新，账号细节由用户填充。
