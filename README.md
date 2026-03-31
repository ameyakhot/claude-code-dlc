# claude-code-dlc

Decompiled source of [Claude Code](https://claude.ai/code) — Anthropic's AI-powered CLI for software engineering. This repository contains the TypeScript source extracted from Claude Code's bundled distribution for study and reference.

> **Disclaimer:** This is an unofficial decompilation. All rights belong to Anthropic. This repository is for educational and research purposes only.

## What is Claude Code?

Claude Code is an interactive terminal-based AI assistant that helps developers write, debug, review, and refactor code. It provides a rich REPL interface where users collaborate with Claude through natural language, while Claude can directly read files, edit code, run shell commands, search codebases, and manage development workflows — all within the terminal.

## Architecture Overview

The application is built with **TypeScript** on the **Bun** runtime, using **React** and a custom fork of **Ink** for terminal UI rendering, with **Yoga** for layout calculation.

```
src/
  main.tsx                 # Application entry point
  QueryEngine.ts           # Core conversation & tool invocation engine
  commands.ts              # Slash command registry (100+ commands)
  Tool.ts                  # Base tool definitions and permission model
  context.ts               # System prompt and context assembly
  interactiveHelpers.tsx    # Interactive dialog and prompt helpers
  cost-tracker.ts          # API usage and cost tracking
  history.ts               # Conversation history management
```

### Core Modules

| Directory | Purpose |
|---|---|
| `entrypoints/` | Launch modes — CLI, SDK, bridge |
| `bootstrap/` | Multi-phase startup pipeline (config, telemetry, API preconnect) |
| `screens/` | Top-level UI screens (REPL, welcome, settings) |
| `components/` | Reusable UI components (prompt input, diff viewer, spinners) |
| `ink/` | Custom Ink fork with layout engine, event system, rendering |
| `native-ts/` | Performance-critical native bridges (color-diff, file-index, yoga-layout) |
| `state/` | Reactive application state with observable store pattern |
| `services/` | Business logic layer (API, analytics, LSP, OAuth, MCP, compaction) |
| `hooks/` | Extensible hook system for tool permissions and notifications |
| `plugins/` | Plugin architecture with marketplace and versioning |
| `skills/` | Reusable AI workflows (commit, review, batch, schedule, etc.) |
| `coordinator/` | Multi-agent orchestration with worker delegation |
| `bridge/` | Remote session execution and management |
| `tasks/` | Background task system (shell, agent, remote, teammate) |
| `memdir/` | Persistent cross-session memory system |
| `assistant/` | Alternative interaction mode (feature-flagged) |
| `buddy/` | Companion/personality system (duck companions with stats) |
| `moreright/` | Split-pane side-by-side view capability |
| `keybindings/` | Keyboard shortcut configuration |
| `outputStyles/` | Output formatting and theming |
| `migrations/` | Settings and data migration handlers |
| `constants/` | Application-wide constants and defaults |

## Tool System

Claude Code exposes a rich set of tools that the AI can invoke during conversations:

**File Operations** — `Read`, `Edit`, `Write`, `Glob`, `Grep`
Read, search, and modify files with fine-grained permission controls.

**Code Execution** — `Bash`, `PowerShell`, `REPL`, `NotebookEdit`
Run shell commands, execute code in Python/Node/Bun REPLs, and edit Jupyter notebooks.

**Code Intelligence** — `LSP`
Language Server Protocol integration for hover info, go-to-definition, and refactoring.

**Web** — `WebSearch`, `WebFetch`
Search the web and fetch/parse web content.

**Multi-Agent** — `Agent`, `SendMessage`, `TaskCreate/List/Get/Update`, `TeamCreate/Delete`
Spawn worker agents, coordinate teams, and manage structured task lists for complex workflows.

**Planning** — `EnterPlanMode`, `ExitPlanMode`
Structured planning mode for designing implementation strategies before writing code.

**Workspace** — `EnterWorktree`, `ExitWorktree`
Git worktree management for isolated agent workspaces.

**Extensibility** — `MCPTool`, `SkillTool`, `RemoteTrigger`, `CronCreate`
Model Context Protocol integration, custom skills, remote triggers, and scheduled agents.

## Execution Modes

Claude Code supports multiple deployment models:

1. **CLI Mode** — Standard interactive terminal REPL (`entrypoints/cli.tsx`)
2. **SDK Mode** — Programmatic integration via `@anthropic-ai/sdk`
3. **Bridge Mode** — Remote session execution with capacity management and JWT auth
4. **Coordinator Mode** — Hierarchical multi-worker orchestration with shared scratchpad
5. **Direct Connect** — Peer-to-peer session connections

## Key Features

### Interactive REPL
Multi-turn conversational interface with streaming responses, vim-mode input, rich diff rendering, and inline permission prompts.

### Multi-Agent Collaboration
Coordinator mode enables spawning worker agents that operate in isolated git worktrees, communicate via message passing, and share knowledge through a scratchpad directory.

### Memory System
Persistent markdown-based memory (`MEMORY.md`) that survives across sessions. Supports auto-memory with semantic categorization (user, feedback, project, reference types) and team memory synchronization.

### Permission & Security Model
Three-tier permission system (`default`, `auto`, `bypass`) with tool-level allow/deny/ask rules, sandbox mode, denial tracking, and enterprise policy limits.

### Plugin & Skill Architecture
- **Plugins** — Modular extensions with marketplace, versioning, and cache management
- **Skills** — Reusable AI workflows invoked via slash commands (bundled: `batch`, `loop`, `schedule`, `verify`, `simplify`, `remember`, etc.)
- **MCP Servers** — External tool providers via Model Context Protocol

### Enterprise Support
SSO/OAuth authentication, remote managed settings, policy limits and governance, audit logging, telemetry (DataDog, OpenTelemetry), GrowthBook feature flags, and MDM support.

## Tech Stack

| Layer | Technology |
|---|---|
| Language | TypeScript (strict) |
| Runtime | Bun |
| UI Framework | React + custom Ink fork |
| Layout Engine | Yoga (native-ts bridge) |
| AI SDK | `@anthropic-ai/sdk` |
| CLI Parsing | Commander.js |
| Syntax Highlighting | Highlight.js |
| Feature Flags | GrowthBook |
| Observability | DataDog, OpenTelemetry |
| Validation | Zod |

## Command System

100+ slash commands organized by category:

- **Git** — `/commit`, `/review`, `/diff`, `/branch`, `/rewind`
- **Session** — `/resume`, `/clear`, `/status`, `/compact`
- **Development** — `/context`, `/help`, `/config`, `/permissions`
- **Utilities** — `/copy`, `/cost`, `/export`, `/color`
- **Collaboration** — `/agents`, `/tasks`, `/teams`
- **Advanced** — `/plan`, `/fast`, `/teleport`, `/bridge`

## Configuration

Claude Code is configured through several mechanisms:

- **`settings.json`** — User preferences (model, permissions, integrations)
- **`keybindings.json`** — Custom keyboard shortcuts
- **`.claude/agents/`** — Custom agent definitions
- **`.claude/skills/`** — Custom skill definitions
- **`.claude/plugins/`** — Plugin configuration
- **`CLAUDE.md`** — Project-specific instructions loaded into context
- **`MEMORY.md`** — Persistent cross-session memory

## License

This repository contains decompiled source code from Claude Code, which is proprietary software owned by Anthropic. This decompilation is provided for educational and research purposes. Refer to [Anthropic's Terms of Service](https://www.anthropic.com/terms) for usage terms.
