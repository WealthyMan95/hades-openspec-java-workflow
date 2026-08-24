---
name: implementation-runner
description: Use when implementing tasks from an OpenSpec change, especially /opsx:apply or natural language requests such as execute plan, start implementation, or apply this change.
---

# Implementation Runner

Implement pending tasks from an OpenSpec change.

## Input

`/opsx:apply` accepts an optional change name:

```text
/opsx:apply
/opsx:apply <change-name>
```

If omitted, infer from conversation context, auto-select the only active change, or run `openspec list --json` and ask the user to select.

Always announce:

```text
Using change: <name>
To override, run /opsx:apply <other-change-name>.
```

## Preconditions

Before implementation:

1. PRD docs exist and are user-confirmed.
2. OpenSpec artifacts exist and are user-confirmed.
3. The selected change is active.
4. `coding-discipline` assumptions, scope, non-goals, verification, and risks are clear.

Do not implement behavior changes before PRD and OpenSpec confirmation.

For Java/Spring Boot/MyBatis/MySQL tasks, apply these companion skills before editing code:

- `coding-discipline` for scope control and verification discipline.
- `java-coding-standard` for Java structure, null safety, exceptions, logging, DTO/VO/PO, and MyBatis basics.
- `springboot-service-patterns` for Controller/Service/Mapper layering, transactions, idempotency, pagination, async jobs, external calls, and observability.
- `springboot-security-review` when APIs, auth, permissions, sensitive data, imports/exports, SQL injection, CORS/CSRF, or config/secrets are involved.
- `mysql-db-guard` and `sql-performance-review` when MySQL, Mapper, SQL, lists/search/export, pagination, sorting, indexes, or EXPLAIN evidence are involved.
- `codegraph-context-guard` when `.codegraph/` exists or impact analysis is needed.

## Procedure

1. Run:

```bash
openspec status --change "<name>" --json
```

Parse `schemaName` and task artifact information.

2. Run:

```bash
openspec instructions apply --change "<name>" --json
```

3. Handle states:

| State | Behavior |
| --- | --- |
| `blocked` | Show blocker, suggest `/opsx:continue`, stop. |
| `all_done` | Congratulate, suggest `/opsx:archive`, stop. |
| implementable | Continue. |

4. Read every file listed under `contextFiles`. Do not assume file names.
5. Show schema, progress, remaining tasks, and dynamic instruction.
6. Loop through pending tasks until done or blocked.
7. For each task: announce it, make focused changes, verify when practical, then mark `- [ ]` as `- [x]` immediately.
8. When build/test fails, use `java-build-fix` for minimal build recovery.
9. After code changes are complete and before final verification, use `post-implementation-check`.
10. If the self-check returns `NEEDS_SYNC`, `NEEDS_FIX`, or `NEEDS_EVIDENCE`, address that result before continuing.
11. Before declaring implementation complete, use `java-test-strategy` to confirm test coverage and manual verification evidence.

## Pause Conditions

Pause when:

- task is unclear
- design issue appears
- PRD and OpenSpec no longer match the conversation
- context files are missing
- OpenSpec reports blocked state
- tests or build fail in a scope-changing way
- CodeGraph impact, SQL EXPLAIN, MySQL safety evidence, or security review is required but missing
- post-implementation self-check reports `NEEDS_SYNC`, `NEEDS_FIX`, or `NEEDS_EVIDENCE`
- user interrupts

Suggest updating PRD docs and OpenSpec artifacts if implementation reveals design issues.

## Completion Output

```text
## Implementation Complete

**Change:** <change-name>
**Schema:** <schema-name>
**Progress:** <complete>/<total> tasks complete ✓

### Completed This Session
- [x] Task 1

All tasks complete, post-implementation self-check passed, and verification is ready for Review Gate.
```

Before this output, run `post-implementation-check`, then final verification. After this output, enter the review gate.
