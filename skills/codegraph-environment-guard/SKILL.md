---
name: codegraph-environment-guard
description: Use when CodeGraph CLI, MCP wiring, or project indexing may be missing, before using CodeGraph for code navigation, impact analysis, call chains, symbol lookup, or when helping install or initialize CodeGraph on macOS, Linux, or Windows.
---

# CodeGraph Environment Guard

Verify that CodeGraph can be used before relying on CodeGraph impact, call-chain, or symbol evidence.

## Required Boundary

Do not silently install CodeGraph, wire it into agents, upgrade it, or initialize a repository. These actions change the user's machine, Codex/agent configuration, or project files, so they require explicit user approval.

If CodeGraph is missing or the project is not indexed, explain the exact command that will run and wait for a clear yes before executing it. If the user declines, stop CodeGraph setup and continue only with an explicit manual-tracing downgrade.

## Preflight

Before using CodeGraph for Java/Spring/MyBatis navigation or impact evidence, check the CLI.

macOS/Linux shell:

```bash
command -v codegraph
codegraph --version
```

Windows PowerShell or CMD:

```powershell
where.exe codegraph
codegraph --version
```

If a repository already has `.codegraph/`, confirm the index is usable before trusting it. Prefer available CodeGraph MCP tools; otherwise use the CLI:

```bash
codegraph explore "project overview and current index health"
```

If the CLI or MCP is unavailable, do not claim CodeGraph impact evidence.

## User-Approved Install

After explicit user approval, install CodeGraph with the platform-appropriate official installer.

macOS/Linux shell:

```bash
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh
```

Windows PowerShell:

```powershell
irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 | iex
```

If the team prefers npm or Node.js is already available, use npm instead:

```bash
npm i -g @colbymchenry/codegraph
```

Windows npm:

```powershell
npm.cmd i -g @colbymchenry/codegraph
```

Then open a new terminal or refresh PATH if needed, and verify:

```bash
codegraph --version
```

If install fails because of permissions, do not retry with `sudo`, Administrator PowerShell, npm prefix changes, or PATH edits unless the user explicitly chooses that approach.

## Agent Wiring

Installing the CLI does not automatically connect CodeGraph to Codex or other agents. After explicit user approval, run:

```bash
codegraph install
```

This configures supported agents. Do not run it silently because it edits agent configuration.

## Project Initialization

If the target repository is not indexed and the user wants CodeGraph impact evidence, ask before running:

```bash
cd <your-project>
codegraph init
```

`codegraph init` creates `.codegraph/` and builds the local graph. Do not run it for every repository automatically; indexing is a project decision.

## Upgrade

If CodeGraph is installed but stale or the user asks to update it, ask before running:

```bash
codegraph upgrade
```

Use `codegraph upgrade --check` when the user only wants to check for updates.
