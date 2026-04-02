<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# schemas

## Purpose
JSON Schema definitions used for input validation across tools, commands, and configuration. Provides schema objects that validate user input, tool parameters, and configuration files before processing.

## Key Files

| File | Description |
|------|-------------|
| `schemas.ts` | JSON Schema definitions for tools, commands, and configuration validation |

## For AI Agents

### Working In This Directory
- Single-file module with all JSON Schema definitions
- Used by `ToolInputJSONSchema` in tool implementations
- Schemas validate configuration files in `.claude/` directory
- Referenced by `commands/config/` for settings validation
