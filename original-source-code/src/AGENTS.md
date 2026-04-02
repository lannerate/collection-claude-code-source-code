<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# src — Claude Code Source

## Purpose
The complete source code of Claude Code (v2.1.88), decompiled from `@anthropic-ai/claude-code`. A ~163,000-line TypeScript codebase implementing an AI-powered CLI coding assistant with React/Ink terminal UI, 40+ tools, ~87 slash commands, MCP integration, and multi-agent coordination. Approximately 108 feature-gated modules were dead-code-eliminated during compilation.

## Key Files

| File | Description |
|------|-------------|
| `main.tsx` | CLI entry point (803KB). Commander.js CLI parsing, React/Ink terminal UI bootstrap, REPL loop |
| `query.ts` | Core agent loop (686KB). Orchestrates tool execution, context management, Claude API interactions |
| `QueryEngine.ts` | SDK/headless query lifecycle engine for programmatic access without CLI |
| `Tool.ts` | Tool base class and `buildTool()` factory pattern for tool registration |
| `tools.ts` | Tool registry — `getTools()` function assembling all 40+ tools |
| `commands.ts` | Slash command lifecycle — queue, priority sorting, execution, hooks |
| `context.ts` | Context assembly and management for prompt construction |
| `history.ts` | Session transcript persistence and history tracking |
| `interactiveHelpers.tsx` | Interactive UI helpers for user prompts, permissions, and dialogs |
| `dialogLaunchers.tsx` | React components for launching permission and confirmation dialogs |
| `setup.ts` | Initial project setup, configuration loading, and environment detection |
| `cost-tracker.ts` | Token usage and cost tracking across API calls |
| `ink.ts` | Ink rendering configuration and terminal UI initialization |
| `replLauncher.tsx` | REPL session launcher component |
| `projectOnboardingState.ts` | State management for new project onboarding flow |
| `Task.ts` | Task data model and lifecycle management |
| `tasks.ts` | Task list coordination utilities |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `assistant/` | KAIROS autonomous agent mode (see `assistant/AGENTS.md`) |
| `bootstrap/` | Application bootstrap and initialization (see `bootstrap/AGENTS.md`) |
| `bridge/` | Claude Desktop remote bridge — 31 files for IPC and desktop integration (see `bridge/AGENTS.md`) |
| `buddy/` | Lightweight agent companion system (see `buddy/AGENTS.md`) |
| `cli/` | CLI infrastructure — stdio, transports, handlers (see `cli/AGENTS.md`) |
| `commands/` | ~87 slash command implementations in individual subdirectories (see `commands/AGENTS.md`) |
| `components/` | React/Ink terminal UI components — 113 files, 26 subdirectories (see `components/AGENTS.md`) |
| `constants/` | Application-wide constants and configuration values (see `constants/AGENTS.md`) |
| `context/` | Context collapse and management strategies (see `context/AGENTS.md`) |
| `coordinator/` | Multi-agent coordination primitives (see `coordinator/AGENTS.md`) |
| `entrypoints/` | Application entry points for different modes (CLI, SDK, headless) (see `entrypoints/AGENTS.md`) |
| `hooks/` | 87 React hooks for state, permissions, tools, UI (see `hooks/AGENTS.md`) |
| `ink/` | Ink rendering extensions and custom components (see `ink/AGENTS.md`) |
| `keybindings/` | Keyboard shortcut definitions and vim-style bindings (see `keybindings/AGENTS.md`) |
| `memdir/` | Memory directory management for session persistence (see `memdir/AGENTS.md`) |
| `migrations/` | Database and configuration migration scripts (see `migrations/AGENTS.md`) |
| `moreright/` | Extended right-side UI components (see `moreright/AGENTS.md`) |
| `outputStyles/` | Output formatting style definitions (see `outputStyles/AGENTS.md`) |
| `plugins/` | Plugin system core (see `plugins/AGENTS.md`) |
| `query/` | Query processing pipeline helpers (see `query/AGENTS.md`) |
| `remote/` | Remote mode support — SSH and container connections (see `remote/AGENTS.md`) |
| `schemas/` | JSON schema definitions for tools and configs (see `schemas/AGENTS.md`) |
| `screens/` | Full-screen UI views (help, settings, onboarding) (see `screens/AGENTS.md`) |
| `server/` | Local server for IDE and remote connections (see `server/AGENTS.md`) |
| `services/` | Business logic layer — API client, compact, MCP, analytics (see `services/AGENTS.md`) |
| `skills/` | Skills system — user-invocable commands and prompt templates (see `skills/AGENTS.md`) |
| `state/` | Application state management and persistence (see `state/AGENTS.md`) |
| `tasks/` | Task management system (TodoWrite, task lists) (see `tasks/AGENTS.md`) |
| `tools/` | 40+ tool implementations — file ops, search, bash, MCP, agents (see `tools/AGENTS.md`) |
| `types/` | TypeScript type definitions shared across the codebase (see `types/AGENTS.md`) |
| `upstreamproxy/` | HTTP/HTTPS proxy configuration for API calls (see `upstreamproxy/AGENTS.md`) |
| `utils/` | 298 utility functions — the largest directory (see `utils/AGENTS.md`) |
| `vim/` | Vim mode integration for the input prompt (see `vim/AGENTS.md`) |
| `voice/` | Voice mode support for audio interaction (see `voice/AGENTS.md`) |

## For AI Agents

### Working In This Directory
- This is decompiled source — not directly compilable. 108 feature-gated modules are missing.
- TypeScript strict mode is **disabled** (`"strict": false`)
- Uses `bun:bundle` for conditional imports (stubs in `stubs/` directory)
- Feature flags via `feature()` from `bun:bundle` control subsystems
- React + Ink for terminal UI (JSX in `.tsx` files)
- Heavy use of lazy requires for feature-gated modules
- Session state stored in `.claude/` directory

### Architecture Flow
1. `main.tsx` — CLI parsing + REPL bootstrap
2. `query.ts` — Main agent loop (the heart of the system)
3. `tools.ts` → `tools/` — Tool registration and execution
4. `commands.ts` → `commands/` — Slash command processing
5. `services/compact/` — Context compression when approaching token limits
6. `services/api/` — Claude API client with retry logic

### Key Patterns
- **Tool Registration**: `buildTool()` factory in `Tool.ts`, registered in `tools.ts` via `getTools()`
- **Command Lifecycle**: queue → priority sorting → execution → hooks
- **Context Flow**: `processUserInput()` → `query()` → `fetchSystemPromptParts()` → `StreamingToolExecutor` → `autoCompact()` → `runTools()`
- **Error Handling**: `withRetry()` from `services/api/withRetry.ts`, `FallbackTriggeredError`
- **Permission System**: Three modes (default/bypass/strict), ML-based inference classifier

### Testing
- No test framework in this decompiled source
- Original tests were not included in the npm package

## Dependencies

### External
- React + Ink — Terminal UI rendering
- Commander.js — CLI argument parsing
- `@anthropic-ai/sdk` — Claude API client
- esbuild — Bundling (build time)
- bun — Runtime with `bun:bundle` for conditional imports

<!-- MANUAL: Custom project notes can be added below -->
