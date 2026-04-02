<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# context

## Purpose
React context providers for the Claude Code application. Provides shared state via React context API for queued messages, modal management, notifications, overlays, stats tracking, FPS metrics, voice state, and mailbox (inter-component messaging).

## Key Files

| File | Description |
|------|-------------|
| `QueuedMessageContext.tsx` | Context for queued message management |
| `fpsMetrics.tsx` | FPS performance metrics context |
| `mailbox.tsx` | Inter-component mailbox messaging context |
| `modalContext.tsx` | Modal dialog state management context |
| `notifications.tsx` | Notification state and dispatch context |
| `overlayContext.tsx` | Overlay UI state context |
| `promptOverlayContext.tsx` | Prompt-specific overlay state context |
| `stats.tsx` | Usage statistics context |
| `voice.tsx` | Voice mode state context |

## For AI Agents

### Working In This Directory
- Each file exports a React context provider and a `use` hook
- Contexts are consumed in `main.tsx` and component tree
- `mailbox.tsx` is the primary inter-component communication mechanism
- `modalContext.tsx` manages the stack of permission/request dialogs
- Changes to context shape affect all consumers — check references before modifying
