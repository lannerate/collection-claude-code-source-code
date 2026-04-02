<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# commands

## Purpose
Contains ~87 slash command implementations organized as individual subdirectories. Each command follows a standard structure with its own `index.ts` (main implementation), optional `prompt.ts` (system prompt additions), and `types.ts` (command-specific types). Commands are processed via the lifecycle in parent `commands.ts`: queue → priority sorting → execution → hooks.

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `add-dir/` | Add directory to working context |
| `agents/` | Manage and list available agents |
| `ant-trace/` | Internal tracing/debugging |
| `autofix-pr/` | Automatically fix PR issues |
| `backfill-sessions/` | Backfill historical session data |
| `branch/` | Git branch management |
| `break-cache/` | Clear/ invalidate caches |
| `bridge/` | Claude Desktop bridge management |
| `btw/` | "By the way" — contextual notes |
| `bughunter/` | Automated bug hunting mode |
| `chrome/` | Chrome browser integration |
| `clear/` | Clear conversation/screen |
| `color/` | Color theme configuration |
| `compact/` | Manually trigger context compaction |
| `config/` | View/edit configuration settings |
| `context/` | Context management commands |
| `copy/` | Copy conversation/content to clipboard |
| `cost/` | Display token usage and costs |
| `ctx_viz/` | Context visualization |
| `debug-tool-call/` | Debug tool call execution |
| `desktop/` | Desktop app integration |
| `diff/` | Show file diffs |
| `doctor/` | Diagnose and fix setup issues |
| `effort/` | Set reasoning effort level |
| `env/` | Environment variable management |
| `exit/` | Exit the REPL session |
| `export/` | Export conversation/history |
| `extra-usage/` | Extended usage statistics |
| `fast/` | Toggle fast mode |
| `feedback/` | Submit feedback |
| `files/` | File management operations |
| `good-claude/` | Positive feedback recording |
| `heapdump/` | Memory heap dump for debugging |
| `help/` | Display help and available commands |
| `hooks/` | Manage lifecycle hooks |
| `ide/` | IDE integration settings |
| `install-github-app/` | Install GitHub App integration (14 files) |
| `install-slack-app/` | Install Slack App integration |
| `issue/` | GitHub issue management |
| `keybindings/` | Keyboard shortcut configuration |
| `login/` | Authentication login |
| `logout/` | Authentication logout |
| `mcp/` | MCP server management |
| `memory/` | Memory/context persistence |
| `mobile/` | Mobile device integration |
| `mock-limits/` | Mock rate limits for testing |
| `model/` | Switch AI model |
| `oauth-refresh/` | Refresh OAuth tokens |
| `onboarding/` | First-time user onboarding |
| `output-style/` | Output formatting style |
| `passes/` | Manage API passes/tokens |
| `perf-issue/` | Report performance issues |
| `permissions/` | Permission management |
| `plan/` | Enter plan mode |
| `plugin/` | Plugin management (17 files) |
| `pr_comments/` | PR comment management |
| `privacy-settings/` | Privacy configuration |
| `rate-limit-options/` | Rate limit configuration |
| `release-notes/` | View release notes |
| `reload-plugins/` | Reload all plugins |
| `remote-env/` | Remote environment management |
| `remote-setup/` | Remote connection setup |
| `rename/` | Rename conversations |
| `reset-limits/` | Reset rate limits |
| `resume/` | Resume previous session |
| `review/` | Code review command |
| `rewind/` | Rewind conversation state |
| `sandbox-toggle/` | Toggle sandbox mode |
| `session/` | Session management |
| `share/` | Share conversation |
| `skills/` | Skills management |
| `stats/` | Usage statistics |
| `status/` | Show current status |
| `stickers/` | Sticker/reward system |
| `summary/` | Conversation summary |
| `tag/` | Tag conversations |
| `tasks/` | Task list management |
| `teleport/` | Teleport to remote session |
| `terminalSetup/` | Terminal configuration |
| `theme/` | UI theme selection |
| `thinkback/` | Thinkback mode |
| `thinkback-play/` | Thinkback playback |
| `upgrade/` | Upgrade to latest version |
| `usage/` | Usage tracking and display |
| `vim/` | Vim mode toggle |
| `voice/` | Voice mode toggle |

## For AI Agents

### Working In This Directory
- Each command directory contains at minimum `index.ts` with the command implementation
- Optional files: `prompt.ts` (system prompt additions), `types.ts` (types), `test.ts` (tests)
- Commands are registered and dispatched via parent `commands.ts`
- The `install-github-app/` and `plugin/` commands are the largest with 14+ and 17+ files respectively
- Command names map directly to directory names (e.g., `/help` → `help/`)

### Common Patterns
- Commands export an `execute()` function receiving args and context
- Commands can modify system prompt, queue follow-up commands, or trigger side effects
- Priority-based execution ordering for dependent commands
- Hook system allows pre/post command execution
