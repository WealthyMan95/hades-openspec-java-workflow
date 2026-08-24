---
name: post-implementation-check
description: Use after OpenSpec implementation code changes are complete and before final verification or review, especially when checking code against PRD/OpenSpec/tasks, scope drift, documentation sync, Java/Spring/MyBatis/MySQL quality gates, tests, EXPLAIN evidence, or rollback readiness.
---

# Post Implementation Check

实现后自检。目标是在运行最终验证和进入 Review Gate 前，先确认“需求是否做全、是否多做、文档是否需要同步、证据是否足够”。

## 位置

执行顺序固定为：

```text
代码实现完成
-> 实现后自检
-> 最终验证
-> Review Gate
```

本 skill 不替代测试、代码审查或 PR Quality Gate；它负责把进入这些环节前的缺口先暴露出来。

## 输入

读取当前 change 的：

- PRD 文档组：`docs/<中文功能名>/`
- OpenSpec artifacts 和 `contextFiles`
- `tasks.md` 勾选状态
- 本次代码 diff 和变更文件
- 已执行或计划执行的验证命令

## 自检项

### 需求覆盖

- 逐条对照 PRD、OpenSpec specs/design/tasks 与代码改动。
- 确认每个验收标准都有实现或明确不适用原因。
- 确认每个已勾选 task 都有对应代码、配置、测试或文档证据。

### 范围漂移

- 找出未在 PRD/OpenSpec/tasks 中出现的行为变化。
- 找出无关重构、顺手优化、新依赖、新接口、新字段、新状态流转。
- 如果额外改动合理但改变范围，必须先调用 `sync-guard` 更新 PRD/OpenSpec，再继续。

### 文档同步

- 检查实现中发现的新规则、接口字段、DB 约束、安全规则、回滚方案、验证要求是否已写回 PRD 和 OpenSpec。
- PRD/OpenSpec 与代码不一致时，状态为 `NEEDS_SYNC`，不能进入最终验证或 Review Gate。

### Java / Spring / MyBatis / MySQL 质量

按影响面确认是否已应用：

- `java-coding-standard`：命名、分层、DTO/VO/PO、null、异常、日志。
- `springboot-service-patterns`：Controller/Service/Mapper、事务、分页、外部调用。
- `springboot-security-review`：接口、权限、输入、敏感数据、密钥、SQL 注入。
- `sql-performance-review`：Mapper、SQL、列表、搜索、统计、导出、分页、排序、索引。
- `mysql-db-guard`：MySQL MCP、写入、DELETE、DDL、EXPLAIN、影响行数、回滚确认。

### 证据缺口

- 检查是否需要补单测、集成测试、回归测试或手动验证步骤。
- 涉及关键 SQL 时检查是否需要 `EXPLAIN` 或等价执行计划证据。
- 涉及 DB、核心逻辑、权限、状态流转、外部平台写入时检查回滚方案。
- 无法自动验证时，必须写明环境、步骤、预期、实际和未覆盖风险。

## 输出格式

输出一段“实现后自检结果”：

```text
实现后自检结果：
- 需求覆盖：PASS / NEEDS_FIX / NEEDS_SYNC
- 范围漂移：PASS / NEEDS_SYNC / NEEDS_FIX
- 文档同步：PASS / NEEDS_SYNC
- Java/Spring/MyBatis/MySQL 质量：PASS / NEEDS_FIX / NOT_APPLICABLE
- 测试与证据：PASS / NEEDS_EVIDENCE / NOT_APPLICABLE
- 回滚方案：PASS / NEEDS_EVIDENCE / NOT_APPLICABLE
- 结论：可以进入最终验证 / 需要先修复 / 需要先同步 PRD 与 OpenSpec
```

## 进入下一步规则

- `NEEDS_SYNC`：先调用 `sync-guard`，同步 PRD/OpenSpec 后再继续。
- `NEEDS_FIX`：先修复代码或任务勾选，再重新自检。
- `NEEDS_EVIDENCE`：先补测试、EXPLAIN、手动验证或回滚证据。
- 全部通过或明确不适用后，才能运行最终验证并进入 Review Gate。

