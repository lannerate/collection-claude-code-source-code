<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# screens

## Purpose
Full-screen UI views that replace the main REPL display. Each screen handles a complete user interaction flow — diagnostics, conversation resume, and the main REPL interface.

## Key Files

| File | Description |
|------|-------------|
| `REPL.tsx` | Main REPL screen — the primary interaction interface |
| `Doctor.tsx` | Diagnostic screen — health checks, configuration validation |
| `ResumeConversation.tsx` | Session resume screen — select and resume previous conversations |

## For AI Agents

### Working In This Directory
- `REPL.tsx` is the primary screen — it composes the prompt input, message list, and tool output
- `Doctor.tsx` runs diagnostic checks and displays results
- Screens are rendered by the screen manager in the main loop
- Each screen is a full-screen React component that manages its own focus and keybindings
