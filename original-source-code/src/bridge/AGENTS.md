<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# bridge

## Purpose
Claude Desktop remote bridge — enables Claude Code to be controlled from Claude Desktop via IPC. Handles bidirectional messaging, session lifecycle, JWT authentication, permission callbacks, and remote REPL control. The largest infrastructure module with 31 files.

## Key Files

| File | Description |
|------|-------------|
| `bridgeMain.ts` | Main bridge orchestration and lifecycle |
| `bridgeApi.ts` | Public API surface for bridge consumers |
| `bridgeConfig.ts` | Bridge configuration management |
| `bridgeMessaging.ts` | Message protocol between desktop and CLI |
| `bridgePermissionCallbacks.ts` | Permission request/response over bridge |
| `bridgeUI.ts` | Bridge-specific UI components |
| `bridgeStatusUtil.ts` | Connection status utilities |
| `bridgePointer.ts` | Pointer/focus management across bridge |
| `bridgeDebug.ts` | Debug logging and diagnostics |
| `bridgeEnabled.ts` | Feature gate for bridge activation |
| `replBridge.ts` | REPL session bridge transport |
| `replBridgeTransport.ts` | Transport layer for REPL bridge |
| `replBridgeHandle.ts` | Handle management for REPL bridge |
| `initReplBridge.ts` | Bridge initialization |
| `remoteBridgeCore.ts` | Core remote bridge logic |
| `codeSessionApi.ts` | Code session API for remote consumers |
| `createSession.ts` | Session creation and setup |
| `sessionRunner.ts` | Session execution management |
| `jwtUtils.ts` | JWT token creation and validation |
| `workSecret.ts` | Work-specific secret management |
| `trustedDevice.ts` | Trusted device verification |
| `inboundMessages.ts` | Incoming message processing |
| `inboundAttachments.ts` | File attachment handling over bridge |
| `capacityWake.ts` | Capacity-based wake signaling |
| `flushGate.ts` | Message flushing coordination |
| `pollConfig.ts` | Configuration polling |
| `pollConfigDefaults.ts` | Default poll configuration values |
| `envLessBridgeConfig.ts` | Environment-independent config |
| `sessionIdCompat.ts` | Session ID compatibility layer |
| `types.ts` | Bridge type definitions |
| `debugUtils.ts` | Debugging utilities |

## For AI Agents

### Working In This Directory
- Bridge is a feature-gated module — only active when Claude Desktop is connected
- The flow: Desktop sends message → `inboundMessages.ts` processes → `replBridge.ts` routes to CLI → response flows back via `bridgeMessaging.ts`
- JWT auth via `jwtUtils.ts` and `workSecret.ts` secures the connection
- Permission callbacks flow through `bridgePermissionCallbacks.ts`
