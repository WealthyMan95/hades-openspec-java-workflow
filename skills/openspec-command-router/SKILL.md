---
name: openspec-command-router
description: Use when the user asks to generate, continue, inspect, apply, execute, validate, review, archive, or otherwise act on an OpenSpec change using natural language or /opsx commands.
---

# OpenSpec Command Router

Map user intent to OpenSpec CLI actions while preserving Hades gates.

## Natural Language Mapping

| User intent | Action |
| --- | --- |
| `生成 OpenSpec`, `创建计划`, `/opsx:propose` | Use `openspec-planner` after PRD confirmation. |
| `查看计划`, `当前状态`, `status` | Run `openspec status --change "<name>"`. |
| `执行计划`, `开始实现`, `/opsx:apply` | Use `implementation-runner` after PRD and OpenSpec confirmation. |
| `验证计划`, `检查 OpenSpec` | Run `openspec validate <name>` and project tests when applicable. |
| `归档计划`, `/opsx:archive` | Validate first, then run `openspec archive <name> --yes`. |
| `继续计划`, `/opsx:continue` | Continue creating or updating artifacts according to OpenSpec status/instructions. |

## Change Selection

When a change name is omitted:

1. Infer from conversation context if clear.
2. Auto-select if only one active change exists.
3. Otherwise run `openspec list --json` and ask the user to select.

Always announce the selected change and how to override it.

## Gates

- Do not propose before PRD confirmation.
- Do not apply before OpenSpec confirmation.
- Do not archive before implementation and review state are acceptable.
- Do not create GitHub Issues.
