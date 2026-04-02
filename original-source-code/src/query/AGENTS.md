<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# query/

## Purpose
Support modules for the main query loop (`query.ts` at the src root). These four files extract configuration, dependency injection, token budget management, and stop-hook execution from the monolithic query function to improve testability and separation of concerns. The main `query.ts` file (~686KB) is the heart of the system that orchestrates the agent loop, tool execution, and Claude API interactions, while this directory contains the extracted, testable pieces.

## Key Files
| File | Description |
|------|-------------|
| `config.ts` | Immutable `QueryConfig` snapshot taken once at `query()` entry. Captures session ID and runtime gates (streaming tool execution, tool use summaries, fast mode, internal user type). Excludes `feature()` gates which are dead-code-elimination boundaries and must stay inline. |
| `deps.ts` | Dependency injection for the query loop. Defines `QueryDeps` type with 4 injectable dependencies (model call, microcompact, autocompact, UUID). `productionDeps()` returns real implementations; tests inject fakes via this interface. |
| `stopHooks.ts` | Stop-hook execution logic extracted from the main loop. Handles post-response hooks including stop hooks, task-completed hooks, teammate-idle hooks, memory extraction, job classification, and attachment message creation. Integrates with the command queue system. |
| `tokenBudget.ts` | Token budget tracking and continuation decision logic. `BudgetTracker` monitors token consumption per turn and decides whether to continue (with nudge messages) or stop based on completion thresholds and diminishing returns detection. |

## Subdirectories
None.

## For AI Agents

### Working In This Directory
- These files are extracted from `query.ts` for testability. The main query loop in `../query.ts` is the orchestrator; this directory holds the decomposed, testable pieces.
- `deps.ts` is the dependency injection seam. To test query behavior, inject mock implementations via `QueryDeps`.
- `config.ts` values are immutable for the lifetime of a query call -- do not attempt to mutate them.
- `tokenBudget.ts` controls whether the agent loop continues or stops. Changes to thresholds directly affect agent behavior and cost.
- `stopHooks.ts` is the last code that runs after each agent turn. It handles critical post-processing like memory extraction and notification dispatch.
- The separation between `config.ts` (immutable query-level settings) and the mutable state in the main loop is intentional -- future extraction of a pure `(state, event, config) => state` reducer depends on this boundary.

### Common Patterns
- **Dependency injection**: `QueryDeps` pattern allows test files to inject fakes without `spyOn` boilerplate.
- **Immutable configuration**: `QueryConfig` is snapshotted once and never mutated during the query lifecycle.
- **Budget decisions**: `TokenBudgetDecision` is a tagged union (`ContinueDecision | StopDecision`) for clear branching.
- **Feature gate separation**: Runtime gates (Statsig, env vars) go in `QueryConfig.gates`; compile-time `feature()` calls stay inline at guarded blocks for dead-code elimination.
