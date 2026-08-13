# Hades OpenSpec Workflow

Hades OpenSpec Workflow 是一个面向 Codex 的 PRD-first 工作流插件。它把来自 Asana、Notion、用户对话、Figma、Stripe、Cloudflare、GitHub PR、内部数据库等入口的信息，统一推进为中文 PRD、OpenSpec change、确认后的实现、验证、review 和归档。

这个插件不创建 GitHub Issues。GitHub 只用于仓库访问、分支、提交、PR 和代码审查。

## 解决什么问题

- 需求来源很多，但没有统一入口和触发规则。
- AI 容易跳过需求分析，直接改代码。
- PRD、OpenSpec 和聊天里的口头决策容易不一致。
- OpenSpec 的 propose、apply、archive 指令需要能被自然语言触发。
- 实现完成后需要明确停下来提示 review，而不是自动归档。
- 团队成员需要能从 GitHub clone 后安装同一套 Codex workflow。

## 核心流程

```text
需求入口
-> 需求分析
-> docs/<中文功能名>/ 生成拆分 PRD 文档
-> 用户确认 PRD
-> 创建 OpenSpec change
-> 生成 implementation 所需 artifacts
-> 用户确认 OpenSpec
-> 执行实现
-> 验证
-> 提示用户 review
-> 根据用户输入继续修改、审查、完成或归档
```

任何当前对话中的流程、规则、范围、验收标准、接口、数据库、安全、实现或验证要求发生变化，都必须同步更新 PRD 文档和 OpenSpec artifacts。聊天记录不是最终事实来源，持久事实来源是：

```text
docs/<中文功能名>/...
openspec/changes/<change-name>/...
```

## 插件内容

| Skill | 用途 |
| --- | --- |
| `intake-router` | 判断需求来源，决定是否读取 Asana、Notion、Figma、Stripe、Cloudflare、GitHub PR 或内部上下文。 |
| `prd-writer` | 在实现前生成中文 PRD 文档组。 |
| `prd-split-docs` | 按影响范围决定是否创建原型、接口、数据库、技术设计文档。 |
| `sync-guard` | 当需求或规则变化时，同步 PRD 与 OpenSpec。 |
| `openspec-command-router` | 把自然语言和 `/opsx:*` 映射到 OpenSpec CLI 操作。 |
| `openspec-planner` | 执行 `/opsx:propose`，创建 change 并生成 artifacts。 |
| `implementation-runner` | 执行 `/opsx:apply`，按 tasks 实现并更新任务勾选。 |
| `review-gate` | 实现和验证后提示 review，处理反馈，控制归档。 |

在 Codex 中安装后，skill 名称通常会以插件名前缀暴露，例如：

```text
hades-openspec-workflow:prd-writer
hades-openspec-workflow:openspec-planner
hades-openspec-workflow:implementation-runner
```

## 外部平台支持边界

本仓库提供的是 Codex plugin + skills，也就是工作流和操作规则。它不会把 Asana、GitHub、Cloudflare、Notion、Stripe、Figma、Canva 或内部数据库账号直接打包进去。

外部平台读取或写入依赖当前 Codex 环境中已有的 connector、MCP server 或 CLI：

| 平台 | 典型用途 | 是否内置账号能力 |
| --- | --- | --- |
| Asana | 读取任务、需求来源、状态流转 | 否 |
| GitHub | 读仓库、分支、提交、PR、review | 否 |
| Notion | 读取产品文档、同步知识记录 | 否 |
| Figma | 读取设计稿、确认 UI 行为 | 否 |
| Stripe | 读取支付、订阅、退款上下文 | 否 |
| Cloudflare | 检查部署、DNS、Workers、Pages | 否 |
| Canva | 读取或辅助设计资产 | 否 |
| 内部数据库 MCP | 查询内部业务数据、辅助定位 | 否 |

如果 connector 不可用，插件要求 Codex 明确说明缺少的能力，并继续基于用户提供的信息推进 PRD 或暂停需要外部读取的步骤。

## 快速安装

macOS / Linux：

```bash
git clone https://github.com/<owner>/hades-openspec-workflow.git ~/plugins/hades-openspec-workflow
codex plugin add hades-openspec-workflow@personal
```

Windows PowerShell：

```powershell
git clone https://github.com/<owner>/hades-openspec-workflow.git "$env:USERPROFILE\plugins\hades-openspec-workflow"
codex plugin add hades-openspec-workflow@personal
```

首次安装前如果还没有 personal marketplace，请按 [团队安装指南.md](./团队安装指南.md) 创建 `~/.agents/plugins/marketplace.json`。

安装或更新后，重启 Codex App，或在目标项目中新开一个 Codex 会话，让 skills 重新加载。

## 项目接入

目标项目需要安装并初始化 OpenSpec：

```bash
npm install -g @fission-ai/openspec@latest
cd <your-project>
openspec init
openspec config profile
openspec update
```

之后在 Codex 中说：

```text
请使用 Hades OpenSpec Workflow 处理这个需求。先做需求分析并生成 PRD 文档，不要直接写代码。
```

PRD 确认后：

```text
PRD 已确认，请生成 OpenSpec。
```

OpenSpec 确认后：

```text
OpenSpec 已确认，执行计划。
```

完成 review 后：

```text
归档计划。
```

更多提示词见 [使用说明.md](./使用说明.md)。

## GitHub 发布方式

维护者可以把本目录直接作为 GitHub 仓库发布：

```bash
cd ~/plugins/hades-openspec-workflow
git init
git branch -M main
git add .
git commit -m "feat: add Hades OpenSpec workflow plugin"
git remote add origin https://github.com/<owner>/hades-openspec-workflow.git
git push -u origin main
```

仓库根目录必须保留：

```text
.codex-plugin/plugin.json
skills/
README.md
团队安装指南.md
使用说明.md
```

## 更多文档

- [团队安装指南.md](./团队安装指南.md)
- [安装配置指南.md](./安装配置指南.md)
- [使用说明.md](./使用说明.md)
- [流程图.md](./流程图.md)
- [试运行指南.md](./试运行指南.md)
- [工具失败处理.md](./工具失败处理.md)
