<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# state

## Purpose
Application state management using a centralized store pattern. Manages the global application state including conversation history, tool results, model selection, session metadata, and teammate view state. Uses a selector pattern for efficient re-renders.

## Key Files

| File | Description |
|------|-------------|
| `AppState.tsx` | Application state type definition and initial state |
| `AppStateStore.ts` | Store implementation with subscription and update mechanisms |
| `store.ts` | Store creation and exports |
| `selectors.ts` | Memoized selectors for derived state |
| `onChangeAppState.ts` | State change listener utilities |
| `teammateViewHelpers.ts` | Helpers for teammate/multi-agent view state |

## For AI Agents

### Working In This Directory
- State follows a centralized store pattern (similar to Redux/Zustand)
- `selectors.ts` provides memoized access to derived state
- `AppState.tsx` defines the complete state shape — read this to understand available state
- `teammateViewHelpers.ts` manages multi-agent coordination view state
- State updates trigger React re-renders via subscriptions in `AppStateStore.ts`
