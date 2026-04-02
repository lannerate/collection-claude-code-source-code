<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# utils/

## Purpose
The largest directory in the codebase (329 files across ~30 subdirectories). Contains all shared utility functions, helpers, and cross-cutting logic used throughout Claude Code. Ranges from low-level operations (crypto, hashing, file I/O) to high-level orchestration (hooks, permissions, settings, session management). Every other directory in the codebase imports from here.

## Key Files (Top-Level)
| File | Description |
|------|-------------|
| `config.ts` | Global and project configuration management (reading, writing, watching for changes) |
| `auth.ts` | Authentication: API key resolution, OAuth token refresh, AWS STS validation, subscription type detection |
| `hooks.ts` | User-defined shell hook execution system (pre/post tool use, stop hooks, notification hooks) |
| `tokens.ts` | Token usage calculation from API response messages, context window budgeting |
| `messages.ts` | Message creation, normalization, and conversion helpers |
| `systemPrompt.ts` | System prompt assembly for the main agent loop |
| `debug.ts` | Debug logging controlled by `CLAUDE_CODE_DEBUG` environment variable |
| `log.ts` | Error logging to the Claude log directory |
| `errors.ts` | Custom error classes (`ClaudeError`, `AbortError`, `ConfigParseError`) and error utilities |
| `env.ts` | Environment variable access with memoization |
| `envUtils.ts` | Environment utility functions (truthy checks, config home directory, region resolution) |
| `platform.ts` | Platform detection (macOS, Linux, Windows, Termux) |
| `file.ts` | File system helpers (existence checks, safe writes, path normalization) |
| `git.ts` | Git operations (root discovery, commit info, diff generation) |
| `cwd.ts` | Current working directory management |
| `cliArgs.ts` | CLI argument parsing support |
| `tasks.ts` | Task list management (TodoWrite integration) |
| `teammate.ts` | Teammate identification and naming utilities |
| `tokens.ts` | Token counting, usage extraction, and budget calculation |
| `fastMode.ts` | Fast mode (lower-latency model) gating and cooldown management |
| `context.ts` | Context window size lookup per model |
| `diff.ts` | Diff generation and formatting for file edits |
| `crypto.ts` | Cryptographic utilities (hashing, random bytes) |
| `json.ts` | Safe JSON parse/stringify helpers |
| `sleep.ts` | Promise-based sleep utility |
| `memoize.ts` | Memoization helpers |
| `uuid.ts` | UUID generation |
| `hash.ts` | Hashing utilities for content deduplication |
| `path.ts` | Path normalization and comparison utilities |
| `stream.ts` | Stream processing helpers |
| `format.ts` | String formatting utilities |
| `truncate.ts` | Text truncation with ellipsis support |
| `words.ts` | Word counting and text analysis |
| `stringUtils.ts` | General string manipulation utilities |
| `array.ts` | Array helpers (unique, flatten, group-by) |
| `set.ts` | Set operation utilities |
| `yaml.ts` | YAML parsing and serialization |
| `xml.ts` | XML parsing utilities |
| `sanitization.ts` | Output sanitization for security |
| `proxy.ts` | HTTP/SOCKS proxy configuration and agent creation |
| `http.ts` | HTTP request utilities with user agent |
| `ide.ts` | IDE detection and communication (VS Code, JetBrains) |
| `editor.ts` | External editor detection and launching |
| `terminal.ts` | Terminal capability detection (color, hyperlinks, dimensions) |
| `theme.ts` | Terminal theme detection and configuration |
| `imageResizer.ts` | Image resizing for API attachment limits |
| `imageStore.ts` | Image storage and retrieval for session attachments |
| `imageValidation.ts` | Image format/size validation |
| `pdf.ts` | PDF text extraction |
| `notebook.ts` | Jupyter notebook (.ipynb) reading support |
| `ripgrep.ts` | Ripgrep binary discovery and execution |
| `glob.ts` | Glob pattern matching utilities |
| `lockfile.ts` | File-based locking for concurrent access |
| `bufferedWriter.ts` | Buffered file writing for performance |
| `process.ts` | Process management (stdout writing, signal handling) |
| `gracefulShutdown.ts` | Graceful shutdown with cleanup registry |
| `cleanupRegistry.ts` | Centralized cleanup callback registration |
| `backgroundHousekeeping.ts` | Background maintenance tasks |
| `tempfile.ts` | Temporary file creation and cleanup |
| `cachePaths.ts` | Cache directory path resolution |
| `systemDirectories.ts` | System-level directory paths (config, data, cache) |
| `xdg.ts` | XDG Base Directory specification compliance |
| `stats.ts` | Statistics tracking and reporting |
| `statsCache.ts` | Cached statistics for performance |
| `sessionState.ts` | Session state persistence and change notification |
| `sessionStorage.ts` | Session storage with event reader/writer |
| `sessionTitle.ts` | Automatic session title generation |
| `sessionRestore.ts` | Session restoration from persisted state |
| `sessionStart.ts` | Session initialization logic |
| `sessionActivity.ts` | Session activity tracking and idle callbacks |
| `sessionEnvVars.ts` | Session-scoped environment variable management |
| `sessionEnvironment.ts` | Session environment file loading |
| `completionCache.ts` | Shell completion cache generation |
| `handlePromptSubmit.ts` | User input preprocessing before submission |
| `userPromptKeywords.ts` | Keyword extraction from user prompts |
| `claudeCodeHints.ts` | Hint system for Claude Code usage suggestions |
| `exampleCommands.ts` | Example command generation for onboarding |
| `releaseNotes.ts` | Release notes display logic |
| `autoUpdater.ts` | Auto-update checking and version management |
| `localInstaller.ts` | Local installation support |
| `binaryCheck.ts` | Binary dependency checking |
| `doctorDiagnostic.ts` | Diagnostic checks for troubleshooting |
| `preflightChecks.tsx` | Pre-flight validation before operations |
| `statusNoticeDefinitions.tsx` | Status bar notice definitions |
| `statusNoticeHelpers.tsx` | Status bar notice helper utilities |
| `status.tsx` | Status display formatting |
| `displayTags.ts` | Tag display formatting |
| `heatmap.ts` | Activity heatmap generation |
| `treeify.ts` | Tree-structure visualization for file listings |
| `logoV2Utils.ts` | Logo rendering utilities |
| `fpsTracker.ts` | FPS tracking for UI performance |
| `timeouts.ts` | Timeout constants and utilities |
| `signal.ts` | Abort signal composition and management |
| `generators.ts` | Async generator utilities |
| `sequential.ts` | Sequential execution helpers |
| `queueProcessor.ts` | Queue-based task processing |
| `messageQueueManager.ts` | Message queue management for streaming |
| `toolPool.ts` | Tool pool assembly and filtering |
| `toolSearch.ts` | Tool name search and matching |
| `toolErrors.ts` | Tool-specific error types |
| `toolSchemaCache.ts` | Tool JSON schema caching |
| `toolResultStorage.ts` | Tool result persistence |
| `readEditContext.ts` | Context for file read/edit tool operations |
| `fileRead.ts` | File reading with encoding detection |
| `fileReadCache.ts` | Cached file reading for performance |
| `fileStateCache.ts` | File state tracking for change detection |
| `readFileInRange.ts` | Reading specific line ranges from files |
| `fsOperations.ts` | Low-level filesystem operations abstraction |
| `fileHistory.ts` | File edit history tracking |
| `fileOperationAnalytics.ts` | Analytics for file operations |
| `generatedFiles.ts` | Generated file detection and management |
| `commitAttribution.ts` | Git commit attribution logic |
| `attribution.ts` | Content attribution tracking |
| `codeIndexing.ts` | Code indexing for search |
| `transcriptSearch.ts` | Session transcript search |
| `highlightMatch.tsx` | Search result highlighting component |
| `textHighlighting.ts` | Text highlighting utilities |
| `cliHighlight.ts` | CLI output syntax highlighting |
| `markdown.ts` | Markdown rendering utilities |
| `markdownConfigLoader.ts` | Markdown configuration loading |
| `frontmatterParser.ts` | YAML frontmatter parsing from markdown files |
| `hyperlink.ts` | Terminal hyperlink (OSC 8) generation |
| `horizontalScroll.ts` | Horizontal scroll handling for wide output |
| `fullscreen.ts` | Fullscreen terminal mode support |
| `sliceAnsi.ts` | ANSI-aware string slicing (preserves escape codes) |
| `ansiToPng.ts` | ANSI terminal output to PNG conversion |
| `ansiToSvg.ts` | ANSI terminal output to SVG conversion |
| `asciicast.ts` | Asciicast (asciinema) recording format support |
| `exportRenderer.tsx` | Session export rendering |
| `staticRender.tsx` | Static (non-interactive) rendering for headless mode |
| `renderOptions.ts` | Rendering configuration options |
| `ink.ts` | Ink (React for CLI) re-exports and configuration |
| `keyboardShortcuts.ts` | Keyboard shortcut definitions |
| `pasteStore.ts` | Clipboard paste content store |
| `imagePaste.ts` | Image paste handling |
| `screenshotClipboard.ts` | Screenshot from clipboard support |
| `earlyInput.ts` | Early input buffering before full initialization |
| `promptEditor.ts` | Prompt editing with external editor |
| `promptShellExecution.ts` | Shell command execution from prompts |
| `promptCategory.ts` | Prompt categorization |
| `sideQuery.ts` | Side query execution (lightweight API calls without full loop) |
| `sideQuestion.ts` | Side question handling |
| `queryContext.ts` | Query execution context |
| `queryHelpers.ts` | Helper functions for query processing |
| `queryProfiler.ts` | Query performance profiling |
| `profilerBase.ts` | Base profiler utilities |
| `headlessProfiler.ts` | Profiling for headless/SDK mode |
| `startupProfiler.ts` | Startup performance profiling |
| `fpsTracker.ts` | UI frame rate tracking |
| `slowOperations.ts` | Slow operation detection (JSON parse/stringify instrumentation) |
| `combinedAbortSignal.ts` | Composed abort signals from multiple sources |
| `abortController.ts` | Abort controller management |
| `idleTimeout.ts` | Idle timeout detection |
| `agenticSessionSearch.ts` | Session search for agentic mode |
| `conversationRecovery.ts` | Conversation recovery after crashes |
| `crossProjectResume.ts` | Cross-project session resumption |
| `listSessionsImpl.ts` | Session listing implementation |
| `groupToolUses.ts` | Tool use grouping for display |
| `collapseBackgroundBashNotifications.ts` | Background bash notification collapsing |
| `collapseHookSummaries.ts` | Hook summary collapsing |
| `collapseReadSearch.ts` | Read/search result collapsing |
| `collapseTeammateShutdowns.ts` | Teammate shutdown notification collapsing |
| `bundledMode.ts` | Bundled mode detection |
| `managedEnv.ts` | Managed environment detection |
| `managedEnvConstants.ts` | Managed environment constants |
| `subprocessEnv.ts` | Subprocess environment variable assembly |
| `findExecutable.ts` | Executable discovery on PATH |
| `execFileNoThrow.ts` | execFile wrapper that returns result instead of throwing |
| `execFileNoThrowPortable.ts` | Portable execFile (works across platforms) |
| `execSyncWrapper.ts` | Synchronous exec wrapper with error handling |
| `genericProcessUtils.ts` | Generic process management utilities |
| `directMemberMessage.ts` | Direct member communication |
| `embeddedTools.ts` | Embedded tool definitions |
| `advisor.ts` | Advisor/suggestion system |
| `effort.ts` | Effort level configuration |
| `planModeV2.ts` | Plan mode v2 feature flag and logic |
| `plans.ts` | Plan management utilities |
| `ultraplan/` | Ultra-plan system for advanced planning |
| `standaloneAgent.ts` | Standalone agent mode support |
| `undercover.ts` | Undercover mode utilities |
| `activityManager.ts` | Activity lifecycle management |
| `heapDumpService.ts` | Heap dump capture for memory debugging |
| `warningHandler.ts` | Warning capture and display |
| `errorLogSink.ts` | Error log file sink |
| `sdkEventQueue.ts` | SDK event queue for headless mode |
| `unaryLogging.ts` | Unary operation logging |
| `contextAnalysis.ts` | Context window content analysis |
| `contextSuggestions.ts` | Context-aware suggestion generation |
| `analyzeContext.ts` | Context analysis for optimization |
| `which.ts` | Cross-platform `which` implementation |
| `semver.ts` | Semantic version comparison |
| `taggedId.ts` | Tagged ID generation for message deduplication |
| `withResolvers.ts` | Promise.withResolvers polyfill |
| `objectGroupBy.ts` | Object.groupBy polyfill |
| `lazySchema.ts` | Lazy schema evaluation |
| `zodToJsonSchema.ts` | Zod to JSON Schema conversion |
| `classifierApprovals.ts` | ML-based permission classifier approval tracking |
| `classifierApprovalsHook.ts` | Classifier hook for automatic permission decisions |
| `autoModeDenials.ts` | Auto mode denial tracking |
| `argumentSubstitution.ts` | CLI argument substitution |
| `slashCommandParsing.ts` | Slash command extraction from user input |
| `immediateCommand.ts` | Immediate command execution |
| `commandLifecycle.ts` | Command lifecycle events and listeners |
| `modifiers.ts` | Input modifier key handling |
| `intl.ts` | Internationalization utilities |
| `formatBriefTimestamp.ts` | Brief timestamp formatting |
| `peerAddress.ts` | Peer address resolution |
| `caCerts.ts` | CA certificate loading |
| `caCertsConfig.ts` | CA certificate configuration |
| `mtls.ts` | Mutual TLS configuration |
| `apiPreconnect.ts` | API pre-connection for reduced latency |
| `api.ts` | Low-level API request helpers |
| `contentArray.ts` | Content array manipulation for API messages |
| `extraUsage.ts` | Extra usage metric tracking |
| `modelCost.ts` | Model cost calculation per token |
| `thinking.ts` | Extended thinking configuration |
| `tokenBudget.ts` | Token budget continuation logic |
| `workloadContext.ts` | Workload context for agent execution |
| `worktree.ts` | Git worktree utilities |
| `worktreeModeEnabled.ts` | Worktree mode feature flag |
| `getWorktreePaths.ts` | Worktree path resolution |
| `getWorktreePathsPortable.ts` | Portable worktree path resolution |
| `agentContext.ts` | Agent execution context |
| `agentId.ts` | Agent identification utilities |
| `agentSwarmsEnabled.ts` | Agent swarm feature flag |
| `forkedAgent.ts` | Forked agent process management |
| `inProcessTeammateHelpers.ts` | In-process teammate helper functions |
| `teamDiscovery.ts` | Team discovery and membership |
| `teamMemoryOps.ts` | Team memory operations |
| `teammateContext.ts` | Teammate context assembly |
| `teammateMailbox.ts` | Teammate mailbox messaging |
| `mailbox.ts` | Message mailbox implementation |
| `settingsSync` | Settings synchronization support |
| `memoryFileDetection.ts` | Memory file type detection |
| `mcpInstructionsDelta.ts` | MCP instructions delta tracking |
| `mcpOutputStorage.ts` | MCP tool output storage |
| `mcpValidation.ts` | MCP configuration validation |
| `mcpWebSocketTransport.ts` | MCP WebSocket transport |
| `privacyLevel.ts` | Privacy level detection (essential traffic only) |
| `claudeDesktop.ts` | Claude Desktop integration |
| `desktopDeepLink.ts` | Desktop deep link handling |
| `js.ts` | JetBrains IDE detection and communication |
| `browser.ts` | Browser detection and launching |
| `detectRepository.ts` | Git repository detection |
| `gitDiff.ts` | Git diff generation utilities |
| `gitSettings.ts` | Git-related settings management |
| `githubRepoPathMapping.ts` | GitHub repository path mapping |
| `ghPrStatus.ts` | GitHub PR status checking |
| `appleTerminalBackup.ts` | macOS Terminal.app preferences backup |
| `iTermBackup.ts` | iTerm2 preferences backup |
| `cron.ts` | Cron expression parsing |
| `cronJitterConfig.ts` | Cron jitter configuration |
| `cronScheduler.ts` | Cron-based task scheduling |
| `cronTasks.ts` | Cron task definitions |
| `cronTasksLock.ts` | Cron task distributed locking |
| `concurrentSessions.ts` | Concurrent session limit enforcement |
| `autoRunIssue.tsx` | Auto-run issue detection and display |
| `claudeInChrome/` | Claude-in-Chrome browser extension integration |
| `dxt/` | Desktop extension (DXT) support |
| `nativeInstaller/` | Native package installer for macOS/Linux/Windows |
| `powershell/` | PowerShell-specific utilities |
| `processUserInput/` | User input processing pipeline |
| `beforeAfter/` | Before/after comparison utilities |

## Subdirectories
| Directory | Purpose |
|-----------|---------|
| `model/` | Model configuration: provider detection (Anthropic/Bedrock/Vertex), model strings, capabilities, allowlists, deprecation warnings, context window sizes, and model option parsing |
| `permissions/` | Permission system: mode selection (default/bypass/strict), rule parsing, ML-based bash classifier, path validation, dangerous pattern detection, auto mode state, bypass killswitch |
| `settings/` | Settings management: schema validation, change detection, managed paths, MDM integration, tool validation, internal writes, plugin-only policy |
| `hooks/` | Hook lifecycle: async registry, API query hooks, agent/http/prompt hook executors, file change watchers, hook events, post-sampling hooks, session hooks, SSRF guard |
| `shell/` | Shell integration: bash/PowerShell providers, output limits, read-only command validation, default shell resolution, shell tool utilities |
| `git/` | Git helpers: config parsing, filesystem operations, gitignore handling |
| `messages/` | Message utilities: API message mappers, system init messages |
| `mcp/` | MCP utilities: date/time parsing, elicitation validation |
| `memory/` | Memory system: types, versions for CLAUDE.md/memory file management |
| `suggestions/` | Auto-suggestions: command completion, directory completion, shell history, Slack channels, skill usage tracking |
| `computerUse/` | Computer use tool: host adapter, MCP server, executor, gates, input loader, hotkey handling |
| `sandbox/` | Sandbox environment: adapter interface, UI utilities |
| `secureStorage/` | Secure credential storage: macOS Keychain, plaintext fallback, keychain prefetch |
| `background/` | Background process management |
| `filePersistence/` | File persistence: output scanning |
| `teleport/` | Teleport environment selection and management |
| `swarm/` | Swarm/multi-agent: in-process runner, leader permission bridge, reconnection, spawn utilities, team helpers, teammate init/layout/model/prompt |
| `task/` | Task output: disk output, framework, formatting, SDK progress |
| `todo/` | Todo list types |
| `plugins/` | Plugin system: marketplace management, installation, dependency resolution, LSP/MCP integration, blocklist, versioning, schemas, zip caching |
| `skills/` | Skills: change detection |
| `telemetry/` | Telemetry: BigQuery exporter, Perfetto tracing, session tracing, instrumentation, plugin telemetry |
| `deepLink/` | Deep linking: protocol handler, terminal launcher, terminal preference |
| `dxt/` | Desktop extension (DXT) utilities |
| `nativeInstaller/` | Native installer: macOS/Linux/Windows package management |
| `powershell/` | PowerShell-specific utilities |
| `processUserInput/` | User input processing pipeline |
| `beforeAfter/` | Before/after comparison utilities |
| `claudeInChrome/` | Claude-in-Chrome browser extension integration |

## For AI Agents

### Working In This Directory
- This is the shared utility layer -- changes here have wide-reaching impact across the entire codebase.
- Files are generally self-contained modules; prefer importing specific functions over importing entire files.
- Many utilities use `memoize` from lodash or custom memoization for performance; be aware of caching implications.
- Environment variable access goes through `env.ts` and `envUtils.ts` -- never access `process.env` directly in business logic.
- The `hooks/` and `permissions/` subdirectories are complex subsystems with their own patterns; read their files carefully before modifying.
- Platform-specific code should use `platform.ts` detection rather than `process.platform` directly.
- Use `slowOperations.ts` wrappers (`jsonParse`, `jsonStringify`) instead of native JSON methods for instrumentation.

### Common Patterns
- **Feature gating**: `feature()` from `bun:bundle` with lazy `require()` for optional modules.
- **Singleton services**: `DiagnosticTrackingService`, `HeapDumpService` use static `getInstance()` patterns.
- **Lazy loading**: Heavy modules (AWS SDK, audio capture) are dynamically imported to reduce startup time.
- **Cleanup registration**: Use `registerCleanup()` from `cleanupRegistry.ts` for async cleanup on shutdown.
- **Safe I/O**: Use `writeFileSyncAndFlush_DEPRECATED` or async equivalents from `file.ts` for data integrity.
- **Error wrapping**: Wrap all external errors with `ClaudeError` or domain-specific error classes.
- **Path normalization**: Always use `path.ts` and `file.ts` helpers for cross-platform path handling.
