---
name: sync-guard
description: Use when requirements, workflow rules, scope, non-goals, acceptance criteria, API contracts, database rules, state machines, implementation constraints, security rules, review rules, verification rules, PRD docs, or OpenSpec artifacts change during a conversation.
---

# Sync Guard

Keep conversation decisions synchronized with durable documents.

## Durable Sources

- PRD documents under `docs/<中文功能名>/`
- OpenSpec artifacts under `openspec/changes/<change-name>/`

Chat-only decisions are not authoritative.

## Must Sync

If the user changes any of these, update PRD docs and OpenSpec artifacts before continuing:

- workflow rule
- business requirement
- scope or non-goal
- acceptance criterion
- UI behavior
- API contract
- DB/schema/data rule
- state machine
- implementation constraint
- integration rule
- permission/security rule
- review requirement
- verification requirement

## Procedure

1. Identify the affected PRD split documents.
2. Update the matching PRD files.
3. Update corresponding OpenSpec artifacts if a change exists.
4. If OpenSpec does not exist yet, record the decision in PRD and carry it into `/opsx:propose` later.
5. Summarize synchronized files.
6. Continue only after durable docs match the conversation.

If the update changes implementation scope after coding began, pause implementation and ask whether to revise artifacts before continuing.
