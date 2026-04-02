<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# assistant

## Purpose

Implements the KAIROS autonomous agent mode's session history persistence layer. Provides paginated access to past session events via the Anthropic API, enabling autonomous agents to retrieve and review prior conversation turns. Uses OAuth-authenticated API calls with cursor-based pagination for efficient traversal of large session histories.

## Key Files

| File | Description |
|------|-------------|
| `sessionHistory.ts` | Paginated session event fetcher with `fetchLatestEvents()` and `fetchOlderEvents()` API calls. Defines `HistoryPage`, `HistoryAuthCtx`, and `HISTORY_PAGE_SIZE` (100). Handles auth via `prepareApiRequest()` and OAuth headers. |

## Subdirectories

None.

## For AI Agents

### Working In This Directory

- This is a single-file module with no subdirectories. All functionality lives in `sessionHistory.ts`.
- The module depends on `utils/teleport/api.ts` for OAuth header preparation and `constants/oauth.ts` for the base API URL.
- `SDKMessage` is the core event type; pages contain up to 100 events in chronological order.
- Error handling is intentionally silent (returns `null` on failure) with debug logging via `logForDebugging`.
- The `before_id` cursor pattern is used for backward pagination; `anchor_to_latest: true` fetches the most recent page.

### Common Patterns

- **Cursor-based pagination**: `firstId` from each page serves as the `before_id` for the next older page.
- **Auth context pattern**: `createHistoryAuthCtx()` prepares headers and base URL once; reused across all page fetches.
- **Silent failure**: Network/HTTP errors return `null` rather than throwing, allowing callers to gracefully handle unavailable history.
