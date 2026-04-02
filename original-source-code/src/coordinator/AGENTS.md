<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# coordinator

## Purpose

Implements multi-agent coordinator mode primitives for Claude Code's "Tengu" multi-agent architecture. Coordinator mode is a special operational state where the CLI acts as an orchestrator for sub-agents rather than a single conversational session. It manages the tool allowlist for worker agents, session mode persistence across resume, and the user context (including scratchpad) that workers receive. Feature-gated behind `COORDINATOR_MODE` and the `CLAUDE_CODE_COORDINATOR_MODE` environment variable.

## Key Files

| File | Description |
|------|-------------|
| `coordinatorMode.ts` | Core coordinator mode logic (~500 lines). Provides `isCoordinatorMode()` (env flag check), `matchSessionMode()` (session resume mode synchronization), `getCoordinatorUserContext()` (assembles worker context with scratchpad), and `getCoordinatorToolFilter()` (restricts available tools for workers). Defines internal tool allowlists for worker vs. planner agents. |

## Subdirectories

None.

## For AI Agents

### Working In This Directory

- Coordinator mode is dual-gated: both `feature('COORDINATOR_MODE')` (compile-time) and `CLAUDE_CODE_COORDINATOR_MODE` env var (runtime) must be enabled.
- `matchSessionMode()` flips the env var when resuming a session whose stored mode differs from the current mode. This ensures tool filters and system prompts match the original session behavior.
- The scratchpad feature is checked via a GrowthBook gate (`tengu_scratch`) duplicated locally to avoid circular dependencies with the main filesystem utilities.
- Worker agents get a restricted tool set (excluded: TeamCreate, TeamDelete, SendMessage, SyntheticOutput). Planner agents may have a different allowlist.
- The `isScratchpadGateEnabled()` function is intentionally duplicated from `utils/permissions/filesystem.ts` to break a circular dependency cycle.

### Common Patterns

- **Dual feature gating**: Compile-time `feature()` check + runtime env var check pattern used across the codebase for mode-specific features.
- **Session mode persistence**: Mode stored in session metadata; `matchSessionMode()` reconciles on resume.
- **Tool filtering**: `getCoordinatorToolFilter()` returns a predicate function that restricts the tool registry for sub-agents.
