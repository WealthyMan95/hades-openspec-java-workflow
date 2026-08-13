# 数据库 MCP 配置指南

Hades OpenSpec Workflow 支持内部数据库 MCP 的规则和模板，但不会把真实数据库账号、host、密码或 token 打包进 GitHub 仓库。

发布到 GitHub 前可以保留：

```text
.mcp.example.json
.env.example
skills/db-query-guard/SKILL.md
```

不要提交：

```text
.mcp.json
.env
真实数据库 URL
生产账号密码
SSH tunnel 密钥
```

## 你需要补充的真实信息

这些信息只在安装后本地配置，不写进仓库：

| 项目 | 示例 | 是否进 GitHub |
| --- | --- | --- |
| 数据库类型 | MySQL / Postgres / SQL Server | 可写文档 |
| host / port | `db.example.internal:3306` | 不建议 |
| database name | `orders` | 视敏感程度 |
| readonly username | `ai_readonly` | 不建议 |
| password | `******` | 绝对不进 |
| SSL / tunnel | VPN / SSH / Cloud SQL Proxy | 不进 |
| 权限边界 | 只读 / 受控写 / 禁止 DDL | 可写文档 |

## 推荐账号策略

至少准备一个只读账号：

```text
ai_readonly
```

权限：

```text
SELECT
SHOW
DESCRIBE
EXPLAIN
```

如确实需要写操作，单独准备写账号：

```text
ai_writer
```

写账号建议只给目标库和必要表权限，不给：

```text
DROP
TRUNCATE
GRANT OPTION
SUPER
FILE
```

## 本地 MCP 配置

复制模板：

```bash
cd ~/plugins/hades-openspec-workflow
cp .mcp.example.json .mcp.json
```

复制环境变量模板：

```bash
cp .env.example .env
```

把 `.env` 中的示例连接串替换成本地真实值。`.env` 已被 `.gitignore` 忽略，不应提交。

如果你的 Codex 或 MCP 启动方式不会自动读取 `.env`，请把变量写入 shell profile、系统密钥管理器或 Codex 支持的本地 secret 配置中。

## Postgres 示例

`.mcp.example.json` 默认演示 Postgres MCP：

```json
{
  "mcpServers": {
    "internal-db-readonly": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "${HADES_DB_READONLY_URL}"
      }
    }
  }
}
```

示例环境变量：

```bash
HADES_DB_READONLY_URL=postgresql://readonly_user:password@localhost:5432/app_db
```

## MySQL 示例

如果团队使用 MySQL，请将 `.mcp.json` 改成你选定的 MySQL MCP server。不同 server 的包名和参数不完全相同，按该 server 文档填写。

建议保留同样的环境变量风格：

```bash
HADES_MYSQL_READONLY_URL=mysql://readonly_user:password@localhost:3306/app_db
```

## 查询安全规则

默认允许：

```sql
SELECT
SHOW
DESCRIBE
EXPLAIN
```

默认要求限制：

- 探索查询必须加 `LIMIT`。
- 避免 `SELECT *`。
- PII、token、密钥、支付敏感信息必须脱敏或不查。
- 输出给用户时优先总结，不直接贴大批量数据行。

写操作必须先确认：

```text
目标：
SQL：
影响范围：
预计影响行数：
回滚方式：
验证方式：
```

`DELETE` 必须先 `SELECT COUNT(*)` 和样本预览。

默认拒绝：

```sql
DROP
TRUNCATE
ALTER ... DROP
GRANT
REVOKE
```

## 与 PRD / OpenSpec 的关系

数据库查询结果如果改变了需求理解，必须同步：

```text
docs/<中文功能名>/<功能名>需求PRD.md
docs/<中文功能名>/<功能名>数据库设计.md
docs/<中文功能名>/<功能名>技术设计.md
openspec/changes/<change-name>/...
```

数据库不是绕过流程的捷径，只能作为需求分析、影响面判断、实现验证或 review 证据。
