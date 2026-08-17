---
name: intake-router
description: Use when a request may come from Asana, Notion, user conversation, Figma, Stripe, Cloudflare, GitHub PRs, or internal context and should be routed through PRD, OpenSpec, implementation, review, or archive. Also use when deciding whether to read external context.
---

# Intake Router

Route incoming work into the Hades OpenSpec Workflow.

## Project Bias

For Java/Spring Boot/MyBatis/MySQL projects, Hades must also apply the Java delivery guardrails:

- `coding-discipline` before code edits or code review.
- `java-coding-standard` when writing or reviewing Java code.
- `springboot-service-patterns` when designing or implementing backend services.
- `springboot-security-review` when APIs, auth, permissions, sensitive data, imports/exports, SQL, or configs are touched.
- `mysql-db-guard` and `sql-performance-review` when MySQL, MyBatis, Mapper, SQL, list/search/export, pagination, sorting, indexes, or EXPLAIN evidence are involved.
- `codegraph-context-guard` when `.codegraph/` exists or impact analysis is needed.
- `java-test-strategy`, `java-backend-review`, and `pr-quality-gate` before PR/merge readiness.

## Core Rule

Use this order for code-changing work:

1. Analyze requirements.
2. Create impact-based PRD documents under `docs/<中文功能名>/`.
3. Ask the user to confirm PRD documents.
4. Create OpenSpec change and required artifacts.
5. Ask the user to confirm OpenSpec artifacts.
6. Implement only after confirmation.
7. Verify.
8. Prompt for review.
9. Archive only after user requests archive.

Do not create GitHub Issues. Use GitHub only for repository access, branches, commits, PRs, and review.

## Source Routing

- Asana URL or task wording: read Asana task first when an Asana connector is available.
- Notion URL or product/background docs: read Notion when available.
- Figma design URL or UI/design request: inspect Figma before implementation.
- Stripe/payment/subscription/refund wording: use Stripe read-only tools first when available.
- Cloudflare/deployment/DNS/Workers/Pages wording: inspect Cloudflare state when available.
- GitHub PR URL: inspect PR. Do not create GitHub Issues.
- Customer/order/user IDs: use approved read-only internal data tools when configured.

If a connector is unavailable, state the missing capability and continue from available context.

## Write Action Gate

Before writing to external systems, deploying, refunding, changing DNS, updating tasks/docs, or modifying code, summarize the intended action and require the appropriate workflow confirmation.
