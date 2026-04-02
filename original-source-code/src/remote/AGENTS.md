<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# remote

## Purpose
Remote mode support — enables Claude Code to operate on remote machines via SSH sessions and WebSocket connections. Manages remote session lifecycle, bidirectional message passing, and permission bridging between local and remote environments.

## Key Files

| File | Description |
|------|-------------|
| `RemoteSessionManager.ts` | Remote session lifecycle — create, connect, disconnect, reconnect |
| `SessionsWebSocket.ts` | WebSocket connection management for remote sessions |
| `remotePermissionBridge.ts` | Bridge permission requests/responses across remote boundary |
| `sdkMessageAdapter.ts` | Adapt SDK messages for remote transport |

## For AI Agents

### Working In This Directory
- Remote mode is feature-gated and requires explicit activation
- `RemoteSessionManager.ts` handles SSH connection setup and teardown
- `SessionsWebSocket.ts` manages persistent WebSocket connections with reconnection
- `remotePermissionBridge.ts` tunnels permission dialogs from remote to local UI
- `sdkMessageAdapter.ts` normalizes message format between local and remote SDKs
