<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# services/

## Purpose
Business logic layer for Claude Code. Houses the core service modules that handle API communication with Claude, context window compaction, Model Context Protocol (MCP) integration, analytics/telemetry, OAuth authentication, LSP client management, voice input, rate limiting, and a variety of cross-cutting concerns like notifications, token estimation, and VCR-based test fixture recording.

## Key Files
| File | Description |
|------|-------------|
| `tokenEstimation.ts` | Token counting for messages via Anthropic API, AWS Bedrock, and Vertex AI; supports thinking blocks |
| `vcr.ts` | Video Cassette Recorder pattern for recording/replaying API responses as test fixtures using SHA-1 hashes |
| `claudeAiLimits.ts` | Claude.ai subscription quota tracking with 5-hour and 7-day rate limit windows, early warnings, and overage handling |
| `notifier.ts` | Cross-platform notification dispatch (terminal bell, osascript, terminal-notifier, notify-send) via configurable channels |
| `voice.ts` | Push-to-talk voice input with native audio capture (cpal) or SoX fallback; 16kHz mono recording |
| `voiceStreamSTT.ts` | Streaming speech-to-text for voice mode |
| `voiceKeyterms.ts` | Key term extraction for voice recognition biasing |
| `diagnosticTracking.ts` | Singleton service tracking LSP diagnostics changes across tool use to measure impact |
| `internalLogging.ts` | Internal structured logging to file for debugging |
| `mockRateLimits.ts` | Rate limit mocking for development and testing |
| `rateLimitMessages.ts` | User-facing rate limit warning and error messages |
| `rateLimitMocking.ts` | Rate limit header processing and mock rate limit injection |
| `mcpServerApproval.tsx` | React component for MCP server trust/approval dialog |
| `preventSleep.ts` | Platform-specific sleep prevention (caffeinate/ systemd-inhibit) during long-running operations |

## Subdirectories
| Directory | Purpose |
|-----------|---------|
| `api/` | Claude API client layer: SDK client construction, streaming model calls, retry logic with exponential backoff, error handling, prompt cache break detection, usage tracking, admin requests, and session ingress |
| `compact/` | Context window compaction: auto-compaction when approaching token limits, micro-compaction, API-based micro-compact, post-compact cleanup, session memory compaction, and grouping heuristics |
| `mcp/` | Model Context Protocol: connection manager (React context), server lifecycle, OAuth flow, channel permissions/allowlists, environment expansion, MCP string utilities, normalization, VS Code SDK MCP integration, and XAA/IDP login |
| `analytics/` | Telemetry pipeline: event logging with PII-safe metadata types, Datadog integration, GrowthBook feature flag evaluation, first-party event logging exporter, and sink with killswitch |
| `oauth/` | OAuth 2.0 authentication: token management, crypto helpers, profile fetching, authorization code listener, and client credential storage |
| `lsp/` | Language Server Protocol: client wrapper, diagnostic registry, server instance/process management, configuration, and passive feedback integration |
| `tools/` | Tool execution orchestration: streaming parallel tool executor, tool execution lifecycle, tool hooks, and tool orchestration |
| `autoDream/` | Automatic dream/consolidation system with configuration, consolidation locking, and consolidation prompts |
| `extractMemories/` | Memory extraction service with extraction logic and prompt templates |
| `tips/` | Tip-of-the-day system: registry, history tracking, and scheduled delivery |
| `plugins/` | Plugin lifecycle management: installation, enabling/disabling, updating, and CLI commands |
| `settingsSync/` | Settings synchronization with remote backend, secret scanning, and type definitions |
| `teamMemorySync/` | Team memory synchronization with secret guard enforcement |
| `remoteManagedSettings/` | Remote-managed enterprise settings with watcher and security checks |
| `policyLimits/` | Enterprise policy limit enforcement types |
| `toolUseSummary/` | Tool use summary generation for conversation context |
| `AgentSummary/` | Agent summary generation module |
| `MagicDocs/` | Magic Docs integration for document processing |
| `PromptSuggestion/` | Prompt suggestion engine with speculation support |
| `SessionMemory/` | Session memory persistence, retrieval, and utility functions |

## For AI Agents

### Working In This Directory
- Services are the business logic backbone. Changes here affect the entire application.
- The `api/` subdirectory is the most critical -- it handles all communication with the Anthropic API, AWS Bedrock, and Google Vertex AI.
- The `compact/` subdirectory manages context window pressure; any changes directly impact conversation length and quality.
- The `mcp/` subdirectory is complex with React context providers; UI changes must preserve the context hierarchy.
- Analytics uses marker types (`AnalyticsMetadata_I_VERIFIED_THIS_IS_NOT_CODE_OR_FILEPATHS`) to enforce that no code or file paths are logged. Respect this pattern.
- The `vcr.ts` module uses SHA-1 hashing of inputs for fixture filenames -- changing hashing logic breaks existing test fixtures.

### Common Patterns
- **Singleton services**: `DiagnosticTrackingService` uses a static `getInstance()` pattern.
- **React context providers**: `MCPConnectionManager` wraps child components with MCP connection context.
- **Feature gating**: Many modules use `feature()` from `bun:bundle` with lazy `require()` for optional subsystems.
- **Retry with backoff**: `api/withRetry.ts` implements exponential backoff with credential refresh and rate limit awareness.
- **Dependency injection**: `query/deps.ts` uses a `QueryDeps` pattern for testability -- services consumed by the query loop can be mocked.
- **Marker types for safety**: Analytics metadata uses phantom types to force explicit verification at each logging call site.
