# VS Code Insiders Settings

This repository contains the user-level settings for **VS Code Insiders** — and it's managed by the very tool it configures.

## What is this?

This is a self-referential configuration repo: **GitHub Copilot** (running inside VS Code Insiders) is used to edit and maintain VS Code's own settings files, including its own Copilot configuration. The AI agent reads, modifies, and commits changes to the settings that govern its own behavior.

## Tracked Files

| File | Purpose |
|------|---------|
| `settings.json` | VS Code user settings (editor, terminal, Copilot agent autonomy, MCP config, etc.) |
| `chatLanguageModels.json` | Language model configuration for Copilot Chat |
| `mcp.json` | MCP (Model Context Protocol) server definitions |
| `prompts/` | Custom prompt files for Copilot |
| `snippets/` | User code snippets |

## What's NOT tracked

- `globalStorage/` — Extension state databases (large, binary, ephemeral)
- `workspaceStorage/` — Per-workspace state
- `History/` — Local edit history
- `sync/` — Settings Sync state
- `.env` — Credentials (sudo password, API keys)

## Setup

```bash
cp .env.example .env
# Edit .env with your credentials
```
