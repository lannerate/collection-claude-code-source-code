<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# types

## Purpose
Shared TypeScript type definitions used across the entire codebase. Defines interfaces and types for commands, hooks, permissions, plugins, text input, IDs, logs, and generated event types.

## Key Files

| File | Description |
|------|-------------|
| `command.ts` | Command system types — command definitions, args, results |
| `hooks.ts` | Hook-related type definitions |
| `ids.ts` | ID types for sessions, messages, tool calls |
| `logs.ts` | Log entry type definitions |
| `permissions.ts` | Permission types — modes, rules, decisions |
| `plugin.ts` | Plugin system type definitions |
| `textInputTypes.ts` | Text input state and event types |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `generated/` | Auto-generated types from event schemas (mostly empty — dead-code-eliminated) |

## For AI Agents

### Working In This Directory
- Types here are imported pervasively — changes cascade widely
- `permissions.ts` is critical for understanding the permission system (default/bypass/strict modes)
- `command.ts` defines the contract for all slash commands
- `plugin.ts` defines the plugin API surface
- The `generated/` subdirectory contained auto-generated types from internal event schemas but was mostly eliminated during compilation
