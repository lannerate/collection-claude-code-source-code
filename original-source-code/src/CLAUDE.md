# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **source code of Claude Code itself** (version 2.1.88), decompiled from the npm package `@anthropic-ai/claude-code`. This is a large-scale TypeScript codebase (~163,000 lines) implementing an AI-powered CLI coding assistant.

**Important**: This code was extracted from a compiled bundle and is not directly compilable. Approximately 108 feature-gated modules were dead-code-eliminated and are missing from this source.

## Build Commands

```bash
# Prepare source and build
npm run build

# Type checking only
npm run check

# Start the CLI (after build)
npm start
```

**Note**: The build uses `esbuild` for bundling and includes a preparation step that sets up stubs for missing modules.

## Core Architecture

### Entry Points

- **`main.tsx`** (803KB) - CLI entry point and REPL bootstrap. Uses Commander.js for CLI parsing and React + Ink for terminal UI.
- **`query.ts`** (686KB) - The core main agent loop. This is the heart of the system that orchestrates tool execution, context management, and Claude API interactions.
- **`QueryEngine.ts`** - SDK/Headless query lifecycle engine. Used for programmatic access without the CLI.

### Key Systems

**Tool System** (`tools/`, `Tool.ts`, `tools.ts`)
- 40+ tools including file operations, code search, bash execution, web access, task management, and MCP integration
- `buildTool()` factory pattern for tool registration
- Each tool is self-contained with its own prompt, input schema, and execution logic

**Command System** (`commands/`, `commands.ts`)
- ~87 slash commands (`/commit`, `/review`, `/session`, etc.)
- Command lifecycle: queue → priority sorting → execution → hooks
- Each command in its own directory with implementation and tests

**Permission System** (`hooks/useCanUseTool.ts`, `types/permission.ts`)
- Three modes: `default` (ask user), `bypass` (auto-allow), `strict` (auto-deny)
- Fine-grained tool-level control
- ML-based permission inference classifier
- Persistent rule storage in global config

**Context Management** (`services/compact/`, `services/contextCollapse/`)
- `autoCompact()` - Automatic context compression when approaching token limits
- Three strategies: reactive compression, micro-compression, trimmed compression
- Context collapsing for token efficiency
- Session transcript persistence

**State Management** (`state/`, `types/`)
- Application-wide state using React hooks
- Session state persisted to `.claude/` directory
- History tracking and replay capabilities

### Module Organization

```
src/
├── cli/              # CLI infrastructure (stdio, structured transports)
├── commands/         # Slash command implementations (~87 commands)
├── components/       # React/Ink terminal UI components
├── tools/            # Tool implementations (40+ tools)
├── services/         # Business logic layer
│   ├── api/          # Claude API client, retry logic, error handling
│   ├── compact/      # Context compression strategies
│   ├── mcp/          # Model Context Protocol integration
│   └── analytics/    # Telemetry and analytics
├── utils/            # Utility functions (331 files)
├── types/            # TypeScript type definitions
├── hooks/            # React hooks (87 hooks)
├── bridge/           # Claude Desktop remote bridge
├── remote/           # Remote mode support
├── coordinator/      # Multi-agent coordination
├── tasks/            # Task management (TodoWrite, task lists)
├── assistant/        # KAIROS autonomous agent mode
├── plugins/          # Plugin system
├── skills/           # Skills system (user-invocable commands)
├── vim/              # Vim mode integration
└── voice/            # Voice mode support
```

## Important Patterns

### Tool Registration Pattern

Tools are registered in `tools.ts` using the `getTools()` function. Each tool:
- Exports a `prompt` property for system prompt generation
- Defines input JSON schema using `ToolInputJSONSchema`
- Implements `buildTool()` function that returns tool configuration

### Command Implementation Pattern

Commands in `commands/` follow this structure:
- `index.ts` - Main command implementation
- `prompt.ts` - System prompt additions
- `types.ts` - Command-specific types
- Executed via `executeCommand()` in the main loop

### Context Flow

1. User input received → `processUserInput()` in `main.tsx`
2. Slash commands extracted and queued
3. `query()` in `query.ts` - main agent loop
4. `fetchSystemPromptParts()` - assemble system prompt
5. `StreamingToolExecutor` - parallel tool execution
6. `autoCompact()` - compress if needed
7. `runTools()` - orchestrate tool calls
8. Results streamed back via `yield SDKMessage`

### Error Handling

- All API calls wrapped in `withRetry()` from `services/api/withRetry.ts`
- `FallbackTriggeredError` for fallback scenarios
- Centralized error logging in `utils/log.ts` and `utils/debug.ts`
- User-friendly error messages for UI, detailed logs for debugging

## Missing Modules

**108 feature-gated modules** are not included in this source (dead-code-eliminated):
- Internal Anthropic infrastructure (~70 modules)
- Feature-gated tools (~20 modules)
- Internal prompt assets (~6 files)

These modules are referenced by `feature()` checks but were removed during compilation. See `claude-code-source-code/README.md` for the complete list.

## Feature Flags

The codebase uses extensive feature flagging via the `feature()` function from `bun:bundle`:
- Internal features use obscure names like `tengu_frond_boric`
- Some features are user-visible (e.g., `FAST_MODE`, `WORKTREE_MODE`)
- Flags control entire subsystems (e.g., `KAIROS`, `COORDINATOR_MODE`)

## Testing

No test framework is configured in this decompiled source. The original Anthropic codebase likely uses a test framework, but test files were not included in the npm package.

## Development Notes

- TypeScript strict mode is **disabled** (`"strict": false` in tsconfig.json)
- Uses `bun:bundle` for conditional imports (stubs provided in `stubs/`)
- React with JSX for terminal UI (Ink)
- Heavy use of lazy requires for feature-gated modules
- Session state stored in `.claude/` directory (location varies by OS)
- MCP (Model Context Protocol) for external tool integration
