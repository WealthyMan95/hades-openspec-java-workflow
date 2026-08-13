---
name: review-gate
description: Use after implementing and verifying an OpenSpec change, when prompting the user for review, running code review, handling review feedback, or deciding whether to revise, continue, or archive.
---

# Review Gate

After implementation and verification, stop for review-oriented user choice.

## Required Summary

Report:

- changed files and behavior
- tasks completed this session
- overall task progress
- verification commands and results
- remaining risks or skipped checks

Then tell the user:

```text
改动已完成并通过验证，现在可以进行 review。你可以让我执行代码审查、根据反馈修改，或准备归档计划。
```

## Review Actions

- If the user requests code review, review bugs, risks, regressions, and missing tests first.
- If the user provides review feedback, validate it before changing code.
- If feedback changes requirements, invoke `sync-guard` before continuing.
- If the user asks to archive, validate and archive through OpenSpec.

Do not archive automatically just because implementation is complete.
