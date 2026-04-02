<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# plugins

## Purpose

Implements the built-in plugin system for Claude Code. Built-in plugins are features that ship with the CLI and can be explicitly enabled or disabled by users via the `/plugin` UI. They differ from bundled skills in that they support multiple components (skills, hooks, MCP servers) and persist their enabled state in user settings. Plugin IDs use the format `{name}@builtin` to distinguish them from marketplace plugins.

## Key Files

| File | Description |
|------|-------------|
| `builtinPlugins.ts` | Plugin registry with `registerBuiltinPlugin()`, `getBuiltinPlugins()` (returns enabled/disabled split), `getBuiltinPluginSkillCommands()` (converts plugin skills to Command objects), and `isBuiltinPluginId()` checker. Manages the `Map<string, BuiltinPluginDefinition>` registry and resolves enabled state from user settings. |
| `bundled/index.ts` | Initialization entry point for built-in plugins. Currently scaffolding only (`initBuiltinPlugins()` is empty) -- no built-in plugins are registered yet. Documents the pattern for adding new built-in plugins. |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `bundled/` | Built-in plugin initialization. Contains `index.ts` with `initBuiltinPlugins()` called at CLI startup. Currently empty scaffolding for future plugin migrations. |

## For AI Agents

### Working In This Directory

- Built-in plugins and bundled skills (`src/skills/bundled/`) serve different purposes. Plugins are user-toggleable via `/plugin` UI; skills are always-available commands compiled into the binary.
- `BuiltinPluginDefinition` type is defined in `types/plugin.ts` (outside this directory).
- The registry is a module-level `Map` -- `clearBuiltinPlugins()` exists for testing.
- `skillDefinitionToCommand()` converts `BundledSkillDefinition` to `Command` with `source: 'bundled'` (not `'builtin'`) to keep skills in the Skill tool's listing and analytics pipeline.
- To add a new built-in plugin: import `registerBuiltinPlugin` in `bundled/index.ts` and call it with the plugin definition inside `initBuiltinPlugins()`.

### Common Patterns

- **Registry pattern**: Module-level `Map` with register/get/clear functions.
- **Enabled state resolution**: User preference > plugin default > `true`.
- **Plugin ID format**: `{name}@builtin` distinguishes from marketplace plugins (`{name}@{marketplace}`).
