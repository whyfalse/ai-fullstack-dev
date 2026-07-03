---
name: 项目理解
description: 当提问涉及现有项目的内部逻辑、功能实现、数据库结构、接口定义或潜在 Bug 原因时，使用此技能。
---

# 项目理解与分析技能

## 分析流程

- 首先识别用户的目的：了解需求、了解接口、了解程序功能、查找bug、其他（自行判断）。
- 识别用户问题中的关键词：功能名、API 路径、表名、类名、方法名、错误现象等。
- 根据相关目录结构，查找并约定对应文件。

## 如何查找？

### 匹配功能模块
- 首先列出`/architecture/module/`下所有功能模块名称，找到可能相关的功能。
- `/architecture/module/<功能模块>/`下存放了前端后端功能描述、测试用例和变更日志。

### 查找接口
- 首先阅读接口目录`/architecture/contracts/apis/catalog.md`，找到相关接口名称。
- 接口详情在`/architecture/contracts/apis/interfaces/`下。
- 接口出入参定义在`/architecture/contracts/apis/params/`下。

### 查找数据库结构
- 首先阅读表目录 `/architecture/contracts/database/catalog.md`，找到相关表。
- 具体表结构在`/architecture/contracts/database/`下。

## 输出

- 根据用户问题，整合查到的信息后回答；引用关键文件路径（相对项目根目录）以便用户复核。
- 若涉及潜在 Bug 或风险，给出根因分析与修复建议，并标明影响的接口/模块/表。
- 若问题超出当前项目信息范围，明确说明缺失的信息，并提示由哪类 Agent（架构师/前端/后端）补全。