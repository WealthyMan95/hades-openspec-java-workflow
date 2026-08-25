---
name: review-gate
description: Use after implementing, post-implementation self-checking, and verifying an OpenSpec change, when prompting the user for review, running code review, handling review feedback, or deciding whether to revise, continue, or archive.
---

# Review Gate

After implementation, `post-implementation-check`, and verification, stop for review-oriented user choice.

## Required Summary

Report:

- changed files and behavior
- tasks completed this session
- overall task progress
- verification commands and results
- post-implementation self-check result
- remaining risks or skipped checks

Then tell the user:

```text
改动已完成、实现后自检已通过并通过验证，现在可以进行 review。你可以让我执行代码审查、根据反馈修改，或准备归档计划。
```

## Review Actions

- If the user requests code review, review bugs, risks, regressions, and missing tests first.
- For Java/Spring Boot/MyBatis code review, apply `java-backend-review`.
- For API/auth/permission/sensitive-data/import-export/config/SQL-injection risk, apply `springboot-security-review`.
- For Mapper/SQL/list/search/statistics/export/pagination/sorting/index changes, apply `sql-performance-review`.
- For MySQL MCP, migrations, write SQL, DELETE/DDL, EXPLAIN, or database evidence, apply `mysql-db-guard`.
- For test evidence and missing coverage, apply `java-test-strategy`.
- For PR readiness, apply `pr-quality-gate` before merge/archive readiness is claimed.
- If the user provides review feedback, validate it before changing code.
- If feedback changes requirements, invoke `sync-guard` before continuing.
- If the user asks to archive, use `openspec-environment-guard`, then validate and archive through OpenSpec.

Do not archive automatically just because implementation is complete.
