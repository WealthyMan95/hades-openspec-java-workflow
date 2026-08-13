---
name: prd-split-docs
description: Use when deciding which PRD split documents to create for a feature based on affected areas such as UI behavior, API contracts, database/schema impact, or backend technical design.
---

# PRD Split Docs

Generate only the split documents that match the actual impact area.

## Always Create

- `<功能名>文档入口.md`
- `<功能名>需求PRD.md`

## Optional Documents

Create these only when affected:

| Document | Create when |
| --- | --- |
| `<功能名>原型行为规则.md` | UI, prototype, interaction, button behavior, display state, error/success/warning text, modal, tab, or empty state changes. |
| `<功能名>前端接口设计.md` | API request/response, frontend-backend contract, error code, routing, gateway, pagination, sorting, filtering, or display-state contract changes. |
| `<功能名>数据库设计.md` | New/modified tables, fields, indexes, unique keys, migrations, data ownership, data retention, audit fields, or DB rollback. |
| `<功能名>技术设计.md` | Backend flow, state machine, idempotency, permission, security, logging, audit, performance, async jobs, external calls, or rollback mechanics. |

Do not create unaffected documents. Instead, list them under `未涉及范围` in the entry document with a short reason.

## Optional Document Headings

### 原型行为规则

```markdown
# <功能名>原型行为规则

## 文档信息
## 页面定位
## 页面结构
## 导航 / Tab / 区块行为
## 按钮行为
## 状态变化
## 空状态
## 弹窗与二次确认
## 成功 / 警告 / 错误提示
## 禁用条件
## 前端展示规则
## 原型中不能照搬的 demo 行为
```

### 前端接口设计

```markdown
# <功能名>前端接口设计

## 文档信息
## 范围与通用约定
## 字段权威来源
## 既有接口复用
## 新增 / 修改接口
### 接口名称
#### 请求路径
#### 请求参数
#### 响应字段
#### 分页 / 排序 / 过滤
#### 状态与按钮展示规则
## Gateway / 路由要求
## 错误码
```

### 数据库设计

```markdown
# <功能名>数据库设计

## 文档信息
## 数据 / DB 影响
## 建表规则
## 新增表
## 修改表
## 字段说明
## 索引
## 唯一键 / 幂等键
## 状态字段
## 审计字段
## 数据迁移
## 回滚影响
```

### 技术设计

```markdown
# <功能名>技术设计

## 文档信息
## 用户 / 系统交互
## 核心业务规则
## 状态模型
## 幂等规则
## 权限边界
## 日志 / 监控
## 审计日志
## 安全要求
## 性能风险
## 回滚方案
```
