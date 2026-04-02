<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# tasks

## Purpose

Implements the task management system for Claude Code's background and foreground task execution. Tasks represent concurrent operations -- shell commands, agent sessions, remote cloud sessions, teammate processes, and dream tasks -- that run alongside the main REPL loop. The system provides unified state types, task lifecycle management (start/stop/kill), UI pill labels for the status bar, and per-type implementations for each task variant.

## Key Files

| File | Description |
|------|-------------|
| `types.ts` | Union type definitions: `TaskState` (all task types) and `BackgroundTaskState` (tasks shown in the status indicator). `isBackgroundTask()` predicate filters running/pending tasks that are explicitly backgrounded. |
| `pillLabel.ts` | `getPillLabel()` generates compact footer-pill labels for the background task indicator (e.g., "2 shells", "1 team", "1 cloud session"). `pillNeedsCta()` determines when to show the "press down to view" call-to-action for ultraplan tasks. |
| `stopTask.ts` | Shared task stop logic used by `TaskStopTool` and SDK stop requests. `stopTask()` validates task existence/running state, calls the type-specific kill function, and handles notification suppression for shell tasks. Throws `StopTaskError` with typed error codes. |
| `LocalMainSessionTask.ts` | Handles backgrounding the main session query (Ctrl+B twice). Continues the query in the background, clears to a fresh prompt, and sends a notification on completion. Reuses `LocalAgentTaskState` with `agentType: 'main-session'`. |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `DreamTask/` | Dream tasks -- speculative background processing. Contains `DreamTask.ts`. |
| `InProcessTeammateTask/` | In-process teammate tasks for multi-agent coordination. Contains `InProcessTeammateTask.tsx` (React component) and `types.ts`. |
| `LocalAgentTask/` | Local agent tasks -- runs a sub-agent query in a background process. Contains `LocalAgentTask.tsx` (~83KB, the largest task implementation). |
| `LocalShellTask/` | Local shell command execution as background tasks. Contains `LocalShellTask.tsx` (~66KB), `guards.ts` (type guards), and `killShellTasks.ts`. |
| `RemoteAgentTask/` | Remote cloud-based agent sessions. Contains `RemoteAgentTask.tsx` (~126KB, the largest file in the tasks system). |

## For AI Agents

### Working In This Directory

- Task types are discriminated unions -- each task type has a `type` field that identifies it. Use the type field for narrowing in switch statements.
- `LocalAgentTask.tsx` and `RemoteAgentTask.tsx` are very large files (83KB and 126KB). They handle complex lifecycle management including transcript recording, abort handling, output streaming, and SDK event emission.
- `stopTask.ts` is the shared kill path. All task types must register a kill handler via `getTaskByType()` from `tasks.ts` (root level).
- The `BackgroundTaskState` union is used by the UI footer indicator; `TaskState` includes all states including completed/failed.
- `isBackgroundTask()` filters out foreground tasks (`isBackgrounded === false`) so they do not appear in the pill indicator.
- `pillLabel.ts` uses the `DIAMOND_OPEN` / `DIAMOND_FILLED` figures from `constants/figures.ts` for remote agent status.

### Common Patterns

- **Discriminated union**: Task types identified by `type` field for exhaustive pattern matching.
- **State machine lifecycle**: Tasks progress through `pending` -> `running` -> `completed`/`failed`/`stopped`.
- **Type-specific implementations**: Each task variant in its own subdirectory with self-contained logic.
- **Kill pattern**: `getTaskByType(task.type).kill(taskId, setAppState)` dispatches to the correct kill handler.
