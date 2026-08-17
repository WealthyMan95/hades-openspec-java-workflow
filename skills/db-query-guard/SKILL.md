---
name: db-query-guard
description: Use when a request involves internal database MCP access, schema inspection, business data lookup, SQL execution, validation queries, database write approval, DELETE/DDL safety, or database evidence for PRD/OpenSpec implementation.
---

# DB Query Guard

Use database access as evidence, not as a shortcut around PRD and OpenSpec.

For Java/Spring Boot/MyBatis/MySQL work, prefer `mysql-db-guard`. This generic guard remains the fallback for non-MySQL internal database MCP access.

## Preconditions

- Use only an approved MCP server, connector, or local read-only tool.
- Never ask the user to paste passwords or secrets into chat.
- Never write secrets into PRD, OpenSpec, code, logs, or committed files.
- Prefer read-only access. Escalate writes only when the workflow and user explicitly require it.

## Read Operations

Allowed without extra confirmation when the MCP/tool is configured:

```sql
SELECT ...
SHOW ...
DESCRIBE ...
EXPLAIN ...
```

Before reading sensitive data, limit fields and rows. Prefer counts, aggregates, masked identifiers, and sampled records.

Default limits:

- Add `LIMIT` to exploratory queries.
- Avoid `SELECT *` unless inspecting a small known table.
- Do not dump customer PII, tokens, secrets, payment details, or credentials.

## Write Operations

`INSERT` and `UPDATE` require a user confirmation summary:

```text
目标：
SQL：
影响范围：
预计影响行数：
回滚方式：
验证方式：
```

Execute only after explicit approval.

## DELETE and DDL

`DELETE` requires a preview query first:

```sql
SELECT COUNT(*) ...
SELECT ... LIMIT 20
```

Then ask for explicit authorization with impact and rollback.

DDL requires strong confirmation. Non-destructive `CREATE` and additive `ALTER` can proceed only after approval.

Reject by default:

- `DROP`
- `TRUNCATE`
- `ALTER ... DROP`
- privilege changes
- disabling constraints
- production bulk destructive changes

## PRD and OpenSpec Evidence

When DB access changes requirement understanding, update durable docs before implementation:

- PRD `需求PRD` for business facts, acceptance criteria, risks.
- `数据库设计.md` for schema, indexes, migration, data ownership, rollback.
- `技术设计.md` for idempotency, state model, audit, performance.
- OpenSpec artifacts if a change exists.

## Output Discipline

Summarize evidence instead of dumping rows:

```text
查询结论：
- 表：
- 条件：
- 样本量 / 计数：
- 对需求或实现的影响：
- 是否需要同步 PRD / OpenSpec：
```

If the MCP server is unavailable, state that clearly and ask the user for exported schema or sampled, redacted data.
