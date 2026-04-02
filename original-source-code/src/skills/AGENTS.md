<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# skills

## Purpose

Implements the skills system for user-invocable slash commands in Claude Code. Skills are prompt-based commands (like `/commit`, `/review`, `/debug`) that can be bundled with the CLI, loaded from user-defined directories (`.claude/skills/`), or discovered from MCP servers. The system handles skill registration, frontmatter parsing, argument substitution, file extraction, and lazy loading of skill content.

## Key Files

| File | Description |
|------|-------------|
| `bundledSkills.ts` | Registration API for skills compiled into the CLI binary. `BundledSkillDefinition` type (name, description, allowedTools, hooks, files, `getPromptForCommand`). `registerBundledSkill()` adds skills to the internal registry and handles file extraction for skills with reference files. |
| `loadSkillsDir.ts` | Disk-based skill loader (~850 lines). Scans `.claude/skills/` directories for Markdown files with frontmatter, parses them into `Command` objects. Handles argument substitution, shell frontmatter execution, effort levels, model selection, and `.mcp.json`-based skill discovery from MCP servers. The main entry point for custom and project-level skills. |
| `mcpSkillBuilders.ts` | Write-once registry that breaks the dependency cycle between MCP skill discovery and `loadSkillsDir`. Provides `registerMCPSkillBuilders()` / `getMCPSkillBuilders()` so MCP servers can create skills without importing `loadSkillsDir.ts` directly. Registration happens at module init, before any MCP server connects. |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `bundled/` | 17 built-in skill implementations: `batch.ts`, `claudeApi.ts`, `claudeApiContent.ts`, `claudeInChrome.ts`, `debug.ts`, `keybindings.ts`, `loop.ts`, `loremIpsum.ts`, `remember.ts`, `scheduleRemoteAgents.ts`, `simplify.ts`, `skillify.ts`, `stuck.ts`, `updateConfig.ts`, `verify.ts`, `verifyContent.ts`, plus `index.ts` (registration entry point). |

## For AI Agents

### Working In This Directory

- Skills follow a three-tier architecture: bundled (compiled into CLI), disk-based (user `.claude/skills/`), and MCP-discovered (from MCP server `.mcp.json`).
- `loadSkillsDir.ts` is the largest and most complex file. It handles frontmatter parsing, argument substitution via `${argName}` syntax, shell command execution in frontmatter, effort/model overrides, and skill file extraction.
- `mcpSkillBuilders.ts` exists solely to break a dependency cycle. Do not merge it with `loadSkillsDir.ts`.
- Bundled skills with `files` property extract reference files to disk on first invocation so the model can `Read`/`Grep` them like disk-based skills.
- The `bundled/` subdirectory contains 17 skill implementations. `index.ts` registers them all at startup.
- `BundledSkillDefinition.getPromptForCommand` is the core function -- it returns `ContentBlockParam[]` that becomes the skill's prompt content.

### Common Patterns

- **Frontmatter-based definition**: Skills defined as Markdown files with YAML frontmatter (name, description, allowedTools, model, hooks, etc.).
- **Registry pattern**: Both bundled skills and MCP builders use module-level registries with register/get functions.
- **Lazy extraction**: Skill reference files are extracted to disk only on first use, not at registration time.
- **Argument substitution**: `${argName}` placeholders in skill content replaced at invocation time via `substituteArguments()`.
