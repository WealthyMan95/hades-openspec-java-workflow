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

Do not implement behavior changes before PRD and OpenSpec confirmation.

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

## Pause Conditions

Pause when:

- task is unclear
- design issue appears
- PRD and OpenSpec no longer match the conversation
- context files are missing
- OpenSpec reports blocked state
- tests or build fail in a scope-changing way
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

All tasks complete! You can archive this change with `/opsx:archive`.
```

Then enter the review gate.
