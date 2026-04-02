<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# entrypoints

## Purpose
Application entry points for different execution modes — CLI, SDK/headless, MCP server, and Agent SDK. Each entry point initializes the appropriate runtime context and launches the main loop.

## Key Files

| File | Description |
|------|-------------|
| `cli.tsx` | CLI entry point — Commander.js setup, argument parsing, REPL launch |
| `init.ts` | Shared initialization logic across entry points |
| `mcp.ts` | MCP server entry point for tool server mode |
| `agentSdkTypes.ts` | Agent SDK type definitions |
| `sandboxTypes.ts` | Sandbox configuration types |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `sdk/` | SDK/headless entry point for programmatic access |

## For AI Agents

### Working In This Directory
- `cli.tsx` is the primary entry point invoked by the `claude` command
- `mcp.ts` starts Claude Code as an MCP tool server
- The `sdk/` subdirectory enables embedding Claude Code in other applications
- `agentSdkTypes.ts` defines the public API contract for the Agent SDK
