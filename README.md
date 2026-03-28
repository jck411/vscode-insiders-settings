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

## Memory & Chat Storage

### VS Code Built-in Chat Logs (always on, read-only by Copilot)

These are written by VS Code for the UI (history panel, edit undo). Copilot never reads them. Cannot be toggled off.

```
~/.config/Code - Insiders/User/workspaceStorage/<workspace-hash>/
├── chatSessions/
│   └── <session-uuid>.jsonl        # Full chat transcript for this session (JSONL)
└── chatEditingSessions/             # Agent edit history (for undo/restore UI)
```

Example for this workspace (`file:///home/jack/.config/Code - Insiders/User`):
```
workspaceStorage/77d33e8f1d25285d14b50187d6d28e7d/chatSessions/656ba23d-1b49-4673-9c29-2a0664354f6e.jsonl
```

---

### `github.copilot.chat.tools.memory.enabled`

Local Markdown files that Copilot can write to and read from. Fully inspectable and editable. Three scopes:

**User memory** — persists across all workspaces, sessions, and conversations:
```
globalStorage/github.copilot-chat/memory-tool/memories/
└── jack-infra.md                   # Example: infra credentials, preferences
```

**Session memory** — scoped to one chat session, cleared when it ends. Folder name is the session UUID base64-encoded:
```
workspaceStorage/77d33e8f1d25285d14b50187d6d28e7d/GitHub.copilot-chat/memory-tool/memories/
└── NjU2YmEyM2QtMWI0OS00NjczLTljMjktMmEwNjY0MzU0ZjZl/   # = base64("656ba23d-...")
    └── plan.md
```

**Repo memory** — persists, scoped to the workspace/repo:
```
workspaceStorage/77d33e8f1d25285d14b50187d6d28e7d/GitHub.copilot-chat/memory-tool/memories/
└── repo/
    └── conventions.md
```

---

### `github.copilot.chat.copilotMemory.enabled` *(preview)*

Remote memory stored on GitHub's servers. Cross-session, cross-workspace, and cross-machine. Not stored locally — no folder to inspect or edit. Copilot writes and recalls it automatically.

---

### Summary

#### `tools.memory.enabled` — Local file-based memory (3 scopes)

| | User memory | Session memory | Repo memory |
|---|---|---|---|
| Survives session end | Yes | **No** (deleted) | Yes |
| Cross-workspace | Yes | No | No (tied to one repo) |
| Cross-machine | No (local files) | No | No |
| You can read/edit it | Yes | Yes | Yes |
| Path | `globalStorage/.../memories/` | `workspaceStorage/.../memories/<base64-session-id>/` | `workspaceStorage/.../memories/repo/` |

#### `copilotMemory.enabled` — GitHub-hosted remote memory

| | |
|---|---|
| Survives session end | Yes |
| Cross-workspace | Yes |
| Cross-machine | Yes (stored on GitHub servers) |
| You can read/edit it | No (managed via GitHub repo settings) |
| Scope | Repository-specific |
| Used by | Copilot coding agent, code review (github.com), Copilot CLI |

## Setup

```bash
cp .env.example .env
# Edit .env with your credentials
```
