# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This repo is **not an application** — it is a Claude Code **skill** (`fullstack-dev-workflow`) plus the template source files that the skill installs into a new project. There is nothing to build, lint, or test here.

When the user invokes `fullstack-dev-workflow`, Claude executes `SKILL.md`, which scaffolds a new AI-driven fullstack project by copying templates from this repo into a target project directory. The scaffolded project (not this repo) is where actual development happens.

## Repository layout — template sources vs. project sources

A critical distinction future Claude instances must internalize:

- **Template sources (read by the skill during init, NOT copied as-is to new projects):**
  - `SKILL.md` — entry point; defines the init flow.
  - `需求分析师Agent.md`, `架构师Agent.md`, `前端开发者Agent.md`, `后端开发者Agent.md` — Agent definitions installed into the new project (the dev-manager space gets 需求分析师 + 架构师; frontend space gets 前端开发者; backend space gets 后端开发者).
  - `dev-manager-ai-space/`, `frontend-ai-space/`, `backend-ai-space/` — each contains `rules/` and `skills/` that get copied into the corresponding space of the new project.
  - `appendix/` — copied verbatim into the new project's `appendix/`.
  - `README.md` — documents the workflow for humans; not used programmatically.

- **Files that exist only locally for this repo's own Claude session:**
  - `.claude/settings.local.json` — personal permission allowlist; not part of the template.

When editing anything under the `*-ai-space/` folders or the `*Agent.md` files, you are editing **templates that will be copied into every new project**. Changes here propagate to all future scaffolds. Be deliberate.

## The three-space architecture

A scaffolded project is split across three directories (the dev-manager space plus the user's existing frontend and backend code repos):

1. **Dev-manager space** (the project root created by the skill): holds requirements, architecture, contracts, dev-tasks, appendix. Houses the 需求分析师 and 架构师 agents. Owns the canonical API and DB contracts.
2. **Frontend space** (user's frontend repo): houses the 前端开发者 agent. Reads contracts/tasks from the dev-manager space via `setting.local.json` → `devManagerPath`.
3. **Backend space** (user's backend repo): houses the 后端开发者 agent. Same pattern, plus uses `db-structure-compare` to diff entity classes against the dev-manager DB contracts.

The two `setting.local.json` files (one in dev-manager recording frontend/backend paths, one in each code repo recording `devManagerPath`) are the **only** link between spaces. Paths are resolved through them, never hardcoded.

## The contract-driven workflow

The four agents form a pipeline. Each is invoked manually by the user; they do not chain automatically.

需求分析师 → 架构师 → (前端开发者 ‖ 后端开发者)

Key invariants enforced by the templates:

- **Contract-first**: 架构师 writes `architecture/contracts/apis/` and `architecture/contracts/database/` **before** frontend/backend `.md` files. Developers must not invent or modify contracts — they flag violations and pause for confirmation.
- **DB independence**: DB schema is modeled on business entities, not API response shapes. API responses are pruned, not raw table dumps. Frontend and backend never share an entity class.
- **Change-log binding**: every module change produces a `change-log/<YYYYMMDD>-<seq>.md` in both `requirements/module/<m>/` and `architecture/module/<m>/`. The dev-task ID is `<模块名称>-<YYYYMMDD>-<seq>`, matching the architecture change-log filename exactly so a task is always traceable to its design change.
- **Task granularity**: 架构师 creates one frontend task + one backend task per module change (not per endpoint/page). Implementation-level breakdown happens inside each developer agent's flow.

## Skill conventions (when editing templates)

- Skills live as `<skill-name>/SKILL.md` under each `*-ai-space/skills/`. The frontmatter `name` and `description` are how agents discover and invoke them — keep them in sync with the folder name.
- Agent files use the standard Claude Code agent frontmatter (`name`, `description`, `tools`, `model`, `color`, `memory`, `skills`). The `skills:` list on an agent must match skill folder names in that agent's space.
- The `appendix/接口契约模板/` and `appendix/数据库契约模板/` are the **strict** format examples for contract files. `architectural-design` SKILL.md explicitly forbids adding extra fields beyond what the templates show — when editing templates, preserve the exact structure.
- Filenames in this repo use Chinese characters (`需求分析师Agent.md`, `架构师思维/`, `接口契约模板/`). On Windows/bash use UTF-8 paths; do not transliterate.

## Common operations

- **Initialize a new project**: user invokes the `fullstack-dev-workflow` skill from a target directory; follow `SKILL.md` exactly. Do not pre-create directories not listed in `dev-manager-ai-space/rules/directory-structure.md`.
- **Edit an agent's behavior**: edit the corresponding `*Agent.md` here (propagates to future scaffolds), or edit the installed copy inside an existing project (affects only that project).
- **Edit a skill's behavior**: edit `*-ai-space/skills/<skill>/SKILL.md` here.
- **Change scaffolded directory structure**: edit `dev-manager-ai-space/rules/directory-structure.md` — it is the single source of truth for what gets created at init time.

There is no test suite and no build step. Validate template changes by re-running the skill in a throwaway directory and inspecting the output.
