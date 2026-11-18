---
name: documentation
description: Generate and maintain product and engineering documentation from source code. Use when asked to document a codebase, trace entrypoints, explain application features or workflows, generate architecture guides, or synchronize documentation after code changes.
---

## Setup

1. Treat the application's invocation directory as the project root. Look for `codebase.yaml` there, not in the installed skill directory.
2. If found, read it and use its configured paths for all commands.
3. If missing, run `init` before other documentation commands. For `init` or `exec`, initialize only once per request; `exec` performs initialization as its first stage.

## Paths and portability

- Resolve bundled `instructions/` and `template/` paths relative to this installed `SKILL.md`. Resolve Markdown links relative to the file containing them. Do not assume the skill is installed inside the application repository.
- Resolve configuration paths relative to the project root. Never write generated documentation into the installed skill directory or modify bundled templates.
- `root` in `codebase.yaml` is the documentation folder. Paths mentioned in command instructions are logical output names: `schema.dbml` uses `schema`; `entrypoint-index.json` uses `index`; `entrypoints/` uses `docs`; `feature-index.json` uses `feature_index`; `features/` uses `features`; `mechanism-index.json` uses `mechanism_index`; `mechanisms/` uses `mechanisms`; `workflow-index.json` uses `workflow_index`; `workflows/` uses `workflows`; `mechanisms/topics/` uses `topics`; `overview.md` uses `product_overview`; and `engineering.md` uses `engineering_overview`.
- Explicit configured paths take precedence. Missing paths default to their logical names beneath `root`. For legacy configurations containing only `index` and `docs`, infer `root` from the parent directory of `index`, preserve those two paths, and derive the remaining defaults beneath that root. All documentation paths must stay inside the selected documentation folder.
- Read `instructions/_orchestration.md` before commands that delegate work. Its sequential fallback applies to every subagent requirement in command files. Use available file-reading, searching, and editing tools; no particular agent CLI is required.

## Commands

Read only the matching command's file and follow its instructions. If the request matches no command, ask which one.

| Command | File | What it does |
|---|---|---|
| `init` | `instructions/init.md` | Initialize documentation and configure automatic synchronization in coding-agent instructions |
| `schema` | `instructions/schema.md` | Generate the application's database schema |
| `entrypoint-index` | `instructions/entrypoint-index.md` | Discover and index application entrypoints |
| `entrypoint` | `instructions/entrypoint.md` | Trace entrypoints and document their behavior |
| `mechanism-index` | `instructions/mechanism-index.md` | Discover and index shared technical mechanisms |
| `mechanism` | `instructions/mechanism.md` | Document shared technical mechanisms |
| `feature-index` | `instructions/feature-index.md` | Discover and organize product capabilities |
| `feature` | `instructions/feature.md` | Document individual product features |
| `workflow-index` | `instructions/workflow-index.md` | Discover technical workflows and link applicable mechanisms |
| `workflow` | `instructions/workflow.md` | Document technical workflows using established mechanisms |
| `product-overview` | `instructions/product-overview.md` | Generate the comprehensive product overview |
| `engineering-overview` | `instructions/engineering-overview.md` | Generate the comprehensive engineering guide |
| `exec` | `instructions/exec.md` | Run the complete documentation pipeline autonomously, in dependency order |
| `sync` | `instructions/sync.md` | Update documentation affected by application changes |
| `explain` | `instructions/explain.md` | Explain application behavior and preserve reusable technical knowledge |
