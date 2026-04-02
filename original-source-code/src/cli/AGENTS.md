<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# cli/

## Purpose
CLI infrastructure for Claude Code. Handles the structured I/O layer for both interactive (terminal) and remote (SDK/WebSocket) modes, subcommand handlers for CLI operations, update management, and transport protocols for bidirectional streaming communication. This directory bridges the gap between the core query engine and the outside world, managing how input arrives and output is delivered.

## Key Files
| File | Description |
|------|-------------|
| `structuredIO.ts` | Base I/O class for SDK mode. Handles bidirectional JSON-RPC-style communication between the SDK client and the query engine, including permission requests, hook callbacks, elicitation, and control message normalization. |
| `remoteIO.ts` | Remote I/O layer extending `StructuredIO` for WebSocket/SSE-based streaming. Supports session tracking, keep-alive, CCR (Claude Code Remote) client integration, and session state/metadata change listeners. |
| `print.ts` | Query dependency resolution and tool pool assembly. Handles downloading user settings, remote managed settings, agent definitions, and building the tool pool for query execution. |
| `exit.ts` | Centralized CLI exit helpers (`cliError` / `cliOk`) that replace the copy-pasted print+exit pattern across ~60 subcommand handlers. Uses `: never` return type for TypeScript control flow narrowing. |
| `update.ts` | `claude update` subcommand handler. Checks for updates, runs diagnostics, detects multiple installations, and manages update via npm/native installer based on installation method. |
| `ndjsonSafeStringify.ts` | JSON serialization for NDJSON transports. Escapes U+2028/U+2029 line terminators to prevent stream-splitting corruption in one-message-per-line protocols. |

## Subdirectories
| Directory | Purpose |
|-----------|---------|
| `handlers/` | CLI subcommand handlers (lazy-loaded). `agents.ts` lists configured agents; `auth.ts` handles login/logout/token refresh; `mcp.tsx` manages MCP server configuration (add/remove/list/health-check); `plugins.ts` handles plugin marketplace operations; `autoMode.ts` dumps and critiques auto-mode classifier rules. |
| `transports/` | Network transport implementations. `WebSocketTransport.ts` is the full-duplex WebSocket client with auto-reconnect and liveness detection; `SSETransport.ts` is a server-sent events transport with POST-back writes; `HybridTransport.ts` combines WebSocket reads with batched HTTP POST writes for performance; `SerialBatchEventUploader.ts` serializes and batches outbound events; `WorkerStateUploader.ts` uploads worker state; `ccrClient.ts` is the Claude Code Remote client for bridge mode; `transportUtils.ts` selects transport type based on URL. |

## For AI Agents

### Working In This Directory
- The I/O layer (`structuredIO.ts`, `remoteIO.ts`) is the boundary between the SDK and the core engine. Changes here affect both interactive and programmatic usage.
- `structuredIO.ts` is complex (~700+ lines) with permission callbacks, hook execution, and control message routing. Read it carefully before modifying.
- Transport files handle network unreliability: reconnection with exponential backoff, liveness timeouts, sleep/wake detection, and permanent close code handling.
- `ndjsonSafeStringify.ts` solves a subtle spec issue (U+2028/U+2029 in JSON); do not replace with bare `JSON.stringify` in transport code.
- `exit.ts` is intentionally centralized to avoid process.exit scattered across the codebase; use `cliError`/`cliOk` instead of raw `process.exit`.
- Handler files in `handlers/` are dynamically imported -- they should not import heavy modules at the top level to preserve fast CLI startup.

### Common Patterns
- **Lazy loading**: Subcommand handlers are dynamically imported only when the corresponding `claude *` command runs.
- **Transport abstraction**: `Transport` interface with multiple implementations (WebSocket, SSE, Hybrid) selected by URL scheme.
- **Reconnection budgets**: Transports have time budgets (10 minutes) and exponential backoff with jitter for reconnection attempts.
- **NDJSON protocol**: All transports use newline-delimited JSON for message framing; `ndjsonSafeStringify` ensures line safety.
- **Bidirectional I/O**: `StructuredIO` reads from an input stream and writes to stdout, supporting both terminal and SDK control channels.
- **Centralized exit**: `cliError`/`cliOk` with `: never` return type for control flow narrowing without trailing returns.
