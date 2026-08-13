---
name: prd-writer
description: Use when turning a requirement, Asana task, Notion note, user conversation, product idea, bug, incident, design request, or technical change into Chinese PRD documentation before OpenSpec or implementation.
---

# PRD Writer

Create detailed Chinese PRD documentation before OpenSpec or code changes.

## Required Output Location

Create PRD documents in the target project root:

```text
<ProjectRoot>/docs/<中文功能名>/
```

Always create:

```text
<功能名>文档入口.md
<功能名>需求PRD.md
```

Create optional split documents only when the requirement touches that area:

```text
<功能名>原型行为规则.md
<功能名>前端接口设计.md
<功能名>数据库设计.md
<功能名>技术设计.md
```

Do not create empty placeholder documents for unaffected areas. In the entry document, record unaffected areas and why they are not included.

## PRD Gate

After writing the PRD document group, stop and ask the user to confirm. Do not create OpenSpec artifacts or modify behavior before PRD confirmation.

## `<功能名>文档入口.md`

Include:

```markdown
# <功能名>文档入口

## 文档清单

## 拆分规则

## 当前状态

## PRD 确认状态

## OpenSpec 信息

## 相关链接

## 未涉及范围
```

## `<功能名>需求PRD.md`

Include:

```markdown
# PRD：<功能名>

## 文档信息

## 文档索引

## 需求探索规则框架

## 基本信息

## 背景

## 目标

## Phase 1 范围

### 本期实现

### 本期不实现

## 需求澄清记录

## 术语表

## 验收标准

### 主流程

### 异常与边界

### 数据/权限/安全

## 风险与回滚

## 待确认问题
```

## Quality Bar

- Mark unknowns as `待确认问题`; do not silently assume fragile business rules.
- Keep implementation out of scope until PRD is confirmed.
- Preserve source links and evidence from Asana, Notion, Figma, Stripe, Cloudflare, GitHub PRs, or user conversation.
- Use concrete acceptance criteria, non-goals, risks, rollback, and owner suggestions.
