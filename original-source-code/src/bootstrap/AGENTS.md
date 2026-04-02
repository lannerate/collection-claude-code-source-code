<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# bootstrap

## Purpose
Application bootstrap and initialization sequence. Handles the startup logic that runs before the main REPL loop — loading configuration, detecting environment, setting up feature flags, and preparing the runtime context.

## Key Files

| File | Description |
|------|-------------|
| `bootstrap.ts` | Bootstrap sequence — config loading, environment detection, feature flag initialization, runtime setup |

## For AI Agents

### Working In This Directory
- Single-file module — `bootstrap.ts` contains the entire startup sequence
- Runs before `main.tsx` enters the REPL loop
- Loads user config, project config, and merges with defaults
- Initializes feature flags from `bun:bundle` feature system
- Sets up global error handlers and logging
