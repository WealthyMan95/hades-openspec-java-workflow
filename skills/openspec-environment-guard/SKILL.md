---
name: openspec-environment-guard
description: Use when OpenSpec CLI may be missing or uninitialized, before running openspec or /opsx commands, or when helping install or initialize Fission AI OpenSpec on macOS, Linux, or Windows.
---

# OpenSpec Environment Guard

Verify that the target environment can run Fission AI OpenSpec before any `openspec` CLI action.

## Required Boundary

Do not silently install OpenSpec or initialize a project. Global npm installs and `openspec init` both change the user's environment or repository, so they require explicit user approval.

If OpenSpec is missing, explain the exact command that will run and wait for a clear yes before executing it. If the user declines, stop OpenSpec work and leave the command for manual execution.

## Preflight

Before `/opsx:propose`, `/opsx:apply`, `/opsx:archive`, `openspec validate`, or any direct `openspec` command, check the CLI.

macOS/Linux shell:

```bash
command -v openspec
openspec --version
```

Windows PowerShell or CMD:

```powershell
where.exe openspec
openspec --version
```

If `openspec` is not found, also check npm.

macOS/Linux shell:

```bash
command -v npm
npm --version
```

Windows PowerShell or CMD:

```powershell
where.exe npm
npm.cmd --version
```

## User-Approved Install

After explicit user approval, install OpenSpec with the platform-appropriate command.

macOS/Linux shell:

```bash
npm install -g @fission-ai/openspec@latest
```

Windows PowerShell or CMD:

```powershell
npm.cmd install -g @fission-ai/openspec@latest
```

Then verify:

```bash
openspec --version
```

If npm is missing, do not install Node.js automatically. Tell the user Node.js/npm is required and ask them to install it through the team's standard method before continuing.

If install fails because of permissions, do not retry with `sudo`, Administrator PowerShell, or npm prefix changes unless the user explicitly chooses that approach.

## Project Initialization

If the target repository is not initialized for OpenSpec, ask before running:

```bash
openspec init
openspec config profile
openspec update
```

Run these commands from the target project root. After initialization, continue the original OpenSpec action.
