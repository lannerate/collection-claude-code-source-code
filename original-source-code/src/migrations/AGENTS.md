<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# migrations

## Purpose
Configuration and settings migration scripts. Each file handles a specific migration — upgrading settings formats, changing default models, migrating feature flags, and ensuring backward compatibility across Claude Code versions.

## Key Files

| File | Description |
|------|-------------|
| `migrateFennecToOpus.ts` | Migrate from Fennec codebase to Opus model |
| `migrateLegacyOpusToCurrent.ts` | Migrate legacy Opus model configuration |
| `migrateOpusToOpus1m.ts` | Migrate Opus to Opus 1M context |
| `migrateSonnet1mToSonnet45.ts` | Migrate Sonnet 1M to Sonnet 4.5 |
| `migrateSonnet45ToSonnet46.ts` | Migrate Sonnet 4.5 to Sonnet 4.6 |
| `migrateAutoUpdatesToSettings.ts` | Migrate auto-update config to settings |
| `migrateBypassPermissionsAcceptedToSettings.ts` | Migrate bypass permissions flag |
| `migrateEnableAllProjectMcpServersToSettings.ts` | Migrate MCP server settings |
| `migrateReplBridgeEnabledToRemoteControlAtStartup.ts` | Migrate REPL bridge config |
| `resetAutoModeOptInForDefaultOffer.ts` | Reset auto-mode opt-in for new offer |
| `resetProToOpusDefault.ts` | Reset Pro tier default to Opus |

## For AI Agents

### Working In This Directory
- Migrations run on app startup, before the main loop
- Each migration is idempotent — safe to run multiple times
- Migrations are ordered by dependency (model migrations chain: Fennec → Opus → Opus 1M → Sonnet chain)
- Add new migrations when changing settings format or default model
