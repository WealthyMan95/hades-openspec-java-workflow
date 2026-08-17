---
name: openspec-planner
description: Use when proposing or creating an OpenSpec change and generating artifacts from confirmed PRD documents, especially for /opsx:propose or user requests like generate OpenSpec or create plan.
---

# OpenSpec Planner

Create an OpenSpec change and generate all artifacts required for implementation.

## Inputs

`/opsx:propose` accepts:

```text
/opsx:propose <kebab-case-change-name>
/opsx:propose <natural-language-description>
```

If no input is provided, ask what the user wants to build or fix. Do not proceed without understanding the change.

## Preconditions

Before creating OpenSpec:

1. Confirm PRD docs exist under `docs/<中文功能名>/`.
2. Confirm the entry document and `需求PRD` exist.
3. Confirm all affected optional PRD split docs exist.
4. Confirm the user approved the PRD document group.

For Java/Spring Boot/MyBatis/MySQL work, ensure the OpenSpec artifacts reflect:

- service layering, transactions, idempotency, pagination, async jobs, external calls, and observability from `springboot-service-patterns`
- Java style, DTO/VO/PO boundaries, null safety, exceptions, logging, and MyBatis constraints from `java-coding-standard`
- auth, authorization, sensitive data, SQL injection, CORS/CSRF, file import/export, secrets, and dependency risks from `springboot-security-review`
- SQL shape, indexes, pagination/sorting, N+1 avoidance, and EXPLAIN evidence expectations from `sql-performance-review`
- MySQL access/write/DDL safety and rollback requirements from `mysql-db-guard`

## Procedure

1. Derive or accept a lowercase kebab-case change name.
2. If the change exists, ask whether to continue it or use another name.
3. Run:

```bash
openspec new change "<name>"
openspec status --change "<name>" --json
```

4. Parse `applyRequires`, artifact statuses, dependencies, and output paths.
5. For each ready artifact in dependency order, run:

```bash
openspec instructions <artifact-id> --change "<name>" --json
```

6. Read dependency artifacts before creating dependent artifacts.
7. Use `template` as the artifact structure and `instruction` as artifact guidance.
8. Treat `context` and `rules` as constraints. Do not copy internal instruction blocks into artifact files.
9. Re-run status after each artifact.
10. Stop when every artifact in `applyRequires` is `done`.
11. Run final human-readable status:

```bash
openspec status --change "<name>"
```

12. Report created artifacts and ask the user to confirm OpenSpec before implementation.

## Output

Summarize:

- change name
- change location
- artifacts created
- readiness for implementation
- prompt: confirm OpenSpec, then run `/opsx:apply` to implement
