<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# server

## Purpose
Local server for IDE and direct connections. Manages WebSocket/HTTP sessions that allow IDE extensions (VS Code, JetBrains) and other clients to connect to Claude Code programmatically. Handles session creation, authentication, and message routing.

## Key Files

| File | Description |
|------|-------------|
| `directConnectManager.ts` | Manage direct IDE-to-CLI connections — lifecycle, auth, routing |
| `createDirectConnectSession.ts` | Create authenticated sessions for direct-connect clients |
| `types.ts` | Server-specific type definitions |

## For AI Agents

### Working In This Directory
- The server enables IDE extensions to control Claude Code
- `directConnectManager.ts` handles the full lifecycle of IDE connections
- Sessions are authenticated via tokens generated in `createDirectConnectSession.ts`
- The server runs alongside the REPL — both active simultaneously
