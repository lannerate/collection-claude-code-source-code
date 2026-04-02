<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# hooks/

## Purpose

87 React hooks providing the behavioral layer for the Claude Code CLI. Hooks encapsulate state management, side effects, and integration logic that drives the terminal UI components. They bridge the gap between the Ink UI framework and the underlying systems -- API interactions, permissions, tool execution, input handling, remote sessions, voice mode, IDE integration, swarm coordination, and more. This directory is the largest single collection of behavioral modules in the codebase, and its hooks are consumed pervasively by components in `../components/` and by the main REPL loop.

## Key Files

### Tool Permissions & Execution

| File | Description |
|------|-------------|
| `useCanUseTool.tsx` | Core permission gate for tool execution; checks classifier-based auto-approval, user-configured rules, and interactive prompts before allowing a tool to run |
| `toolPermission/PermissionContext.ts` | Factory for creating permission context objects used by `useCanUseTool` |
| `toolPermission/permissionLogging.ts` | Logging utilities for permission decisions (accept/deny reasons) |
| `toolPermission/handlers/` | Permission handler strategies: interactive, coordinator, and swarm worker modes |

### REPL & Bridge

| File | Description |
|------|-------------|
| `useReplBridge.tsx` | Initializes and manages the always-on REPL bridge connection for Claude Desktop integration; handles OAuth, message sync, permission forwarding, and auto-reconnect (115KB -- largest hook) |
| `useRemoteSession.ts` | Manages remote SDK sessions; handles incoming/outgoing messages, permission requests, session title updates, and connection lifecycle |
| `useSSHSession.ts` | REPL integration for `claude ssh` sessions; drives an SSH child process with the same interface as `useDirectConnect` |
| `useDirectConnect.ts` | WebSocket-based direct connect hook for remote REPL sessions |
| `useCancelRequest.ts` | Handles cancellation of in-flight API requests and tool executions |

### Input & Typeahead

| File | Description |
|------|-------------|
| `useTypeahead.tsx` | Full typeahead/suggestion system for the prompt input; provides file path completions, slash command suggestions, @-mention resolution, agent suggestions, shell history, and Slack channel completions (212KB -- second largest hook) |
| `useTextInput.ts` | Low-level text input state management; handles cursor movement, kill ring (cut/copy/yank), selection, multiline editing, and input mode switching |
| `useSearchInput.ts` | Search input state for search bars (transcript search, global search); handles query, cursor, and key navigation |
| `useArrowKeyHistory.tsx` | Arrow key navigation through command history with chunked disk loading and mode filtering |
| `useInputBuffer.ts` | Buffers rapid input events to prevent overloading the terminal renderer |
| `usePasteHandler.ts` | Handles paste events including multi-line content and image detection |
| `useVimInput.ts` | Vim modal editing state machine for the text input |
| `useHistorySearch.ts` | Fuzzy history search with debounce for the history dialog |

### Keybindings & Commands

| File | Description |
|------|-------------|
| `useGlobalKeybindings.tsx` | Registers global keyboard shortcuts (Ctrl+T for tasks, Ctrl+O for transcript, Ctrl+E for expand all) |
| `useCommandKeybindings.tsx` | Registers keybinding handlers for slash commands; maps user-configured shortcuts to command invocations |
| `useExitOnCtrlCD.ts` | Two-press Ctrl+C/D exit behavior with configurable timeout |
| `useExitOnCtrlCDWithKeybindings.ts` | Exit-on-Ctrl-C/D with keybinding context integration |
| `useCommandQueue.ts` | Queue processor for slash commands extracted from user input |

### Voice Mode

| File | Description |
|------|-------------|
| `useVoice.ts` | Hold-to-talk voice input hook; manages audio recording, Anthropic voice_stream STT WebSocket, auto-repeat key handling, and language normalization |
| `useVoiceIntegration.tsx` | Integrates voice mode with the REPL; handles voice keybinding activation, rapid-key detection, and voice state transitions |
| `useVoiceEnabled.ts` | Simple flag hook for whether voice mode is enabled |
| `useBlink.ts` | Blinking cursor animation for voice recording indicator |

### IDE Integration

| File | Description |
|------|-------------|
| `useIDEIntegration.tsx` | Initializes IDE extension connections (VS Code, JetBrains); auto-detects terminals and sets up MCP config for IDE features |
| `useIdeAtMentioned.ts` | Tracks whether the user has used @-mention to reference IDE context |
| `useIdeSelection.ts` | Receives and manages the current text selection from the IDE |
| `useIdeConnectionStatus.ts` | Monitors IDE connection health and status |
| `useIdeLogging.ts` | Forwards log messages to the IDE's output channel |
| `useDiffInIDE.ts` | Sends diff views to the IDE for side-by-side comparison |
| `useShowInIDEPrompt.ts` | Prompts user to open files in IDE when available |

### Swarm & Teams

| File | Description |
|------|-------------|
| `useSwarmPermissionPoller.ts` | Polls for permission responses from the team leader when running as a swarm worker agent |
| `useSwarmInitialization.ts` | Initializes swarm/team coordination state |
| `useInboxPoller.ts` | Polls the inbox for teammate messages, permission updates, and plan approvals in multi-agent sessions |
| `useTasksV2.ts` | Manages the V2 task list state for background and in-process tasks |
| `useTaskListWatcher.ts` | Watches for task list changes and triggers UI updates |
| `useTeammateViewAutoExit.ts` | Auto-exits teammate detail view when the task completes |

### Remote & Teleport

| File | Description |
|------|-------------|
| `useTeleportResume.tsx` | Resumes a Teleport session after disconnection or page reload |
| `useSessionBackgrounding.ts` | Handles session background/foreground transitions for long-running operations |
| `useBackgroundTaskNavigation.ts` | Navigation state for switching between background tasks |

### Notifications (notifs/)

| File | Description |
|------|-------------|
| `notifs/useAutoModeUnavailableNotification.ts` | Shows notification when auto/bypass mode is unavailable |
| `notifs/useFastModeNotification.tsx` | Fast mode availability and cooldown notifications |
| `notifs/useRateLimitWarningNotification.tsx` | Rate limit approaching/exceeded warnings |
| `notifs/useModelMigrationNotifications.tsx` | Model deprecation and migration prompts |
| `notifs/useLspInitializationNotification.tsx` | LSP server initialization status |
| `notifs/useMcpConnectivityStatus.tsx` | MCP server connectivity warnings |
| `notifs/usePluginInstallationStatus.tsx` | Plugin install/uninstall progress |
| `notifs/usePluginAutoupdateNotification.tsx` | Plugin auto-update availability |
| `notifs/useDeprecationWarningNotification.tsx` | Feature deprecation notices |
| `notifs/useIDEStatusIndicator.tsx` | IDE connection status indicator |
| `notifs/useInstallMessages.tsx` | Installation progress messages |
| `notifs/useNpmDeprecationNotification.tsx` | npm package deprecation notices |
| `notifs/useSettingsErrors.tsx` | Settings validation error notifications |
| `notifs/useStartupNotification.ts` | Startup-time notifications |
| `notifs/useTeammateShutdownNotification.tsx` | Teammate shutdown/restart notifications |
| `notifs/useCanSwitchToExistingSubscription.tsx` | Subscription upgrade prompt |

### Settings & Config

| File | Description |
|------|-------------|
| `useSettings.ts` | Reads user settings from global/project config |
| `useSettingsChange.ts` | Reacts to settings file changes and triggers re-renders |
| `useDynamicConfig.ts` | Dynamic configuration from remote feature flags |
| `useMainLoopModel.ts` | Tracks the current main loop model (Sonnet, Opus, etc.) |

### Virtual Scroll & Performance

| File | Description |
|------|-------------|
| `useVirtualScroll.ts` | Virtual scrolling engine for the message list; manages mounted range, overscan, scroll quantum quantization, and item height estimation |
| `useAfterFirstRender.ts` | Defers effects until after the first render completes |
| `useMinDisplayTime.ts` | Ensures UI elements display for a minimum duration before transitioning |
| `useMemoryUsage.ts` | Monitors process memory usage for display |

### Suggestions & Files

| File | Description |
|------|-------------|
| `usePromptSuggestion.ts` | Generates context-aware prompt suggestions based on current state |
| `fileSuggestions.ts` | File path completion engine with background cache refresh |
| `unifiedSuggestions.ts` | Unified suggestion ranking across multiple sources |

### Analytics & Surveys

| File | Description |
|------|-------------|
| `useAwaySummary.ts` | Generates a summary of what happened while the user was away |
| `useLogMessages.ts` | Captures and surfaces log messages for the diagnostics view |
| `useSkillImprovementSurvey.ts` | Triggers skill improvement surveys after command usage |
| `useElapsedTimer.ts` | Tracks elapsed time for operations |

### Authentication

| File | Description |
|------|-------------|
| `useApiKeyVerification.ts` | Verifies API key validity and handles re-authentication |

### Miscellaneous

| File | Description |
|------|-------------|
| `useDeferredHookMessages.ts` | Defers hook messages to avoid rendering during transitions |
| `useDiffData.ts` | Computes diff data for file change display |
| `useTurnDiffs.ts` | Tracks per-turn file diffs for the diff review flow |
| `useCopyOnSelect.ts` | Copies selected text to clipboard on selection |
| `useDoublePress.ts` | Detects double-press of a key (e.g., Escape) |
| `useScheduledTasks.ts` | Manages scheduled/recurring task execution |
| `usePrStatus.ts` | Pull request status tracking |
| `useMergedTools.ts` | Merges tools from multiple sources (base + MCP + plugins) |
| `useMergedClients.ts` | Merges MCP client connections |
| `useMergedCommands.ts` | Merges slash commands from multiple sources |
| `useMailboxBridge.ts` | Bridge for inter-process mailbox communication |
| `useChromeExtensionNotification.tsx` | Notification for Chrome extension integration |
| `useOfficialMarketplaceNotification.tsx` | Notification about the official MCP marketplace |
| `usePluginRecommendationBase.tsx` | Base hook for plugin recommendation logic |
| `useLspPluginRecommendation.tsx` | LSP plugin recommendation notifications |
| `useClaudeCodeHintRecommendation.tsx` | Recommends enabling Claude Code hints |
| `useTerminalSize.ts` | Terminal dimensions (columns, rows) |
| `useTimeout.ts` | Declarative timeout hook |
| `useUpdateNotification.ts` | Checks for CLI updates and shows notification |
| `useManagePlugins.ts` | Plugin lifecycle management (install, uninstall, update) |
| `useClipboardImageHint.ts` | Detects images in clipboard and shows hint |
| `useFileHistorySnapshotInit.ts` | Initializes file history snapshots for undo |
| `renderPlaceholder.ts` | Renders placeholder content during loading |
| `useNotifyAfterTimeout.ts` | Sends terminal notification after a timeout |
| `useQueueProcessor.ts` | Generic queue processing hook |
| `useIssueFlagBanner.ts` | Issue flag banner display logic |
| `useSkillsChange.ts` | Reacts to skill configuration changes |
| `useAnimationFrame.ts` | requestAnimationFrame hook for animations |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `toolPermission/` | Permission context creation, handler strategies (interactive, coordinator, swarm worker), and permission decision logging |
| `notifs/` | 16 notification hooks covering auto-mode, rate limits, model migration, MCP connectivity, plugin status, IDE status, and more |

## For AI Agents

### Working In This Directory

- Hooks are standard React hooks (`useEffect`, `useState`, `useCallback`, `useRef`, `useSyncExternalStore`). They follow the React rules of hooks (call at top level, no conditionals).
- The React Compiler runtime (`_c` from `react/compiler-runtime`) is used in many hooks for automatic memoization. The `$ = _c(N)` pattern creates cache slots -- do not modify these.
- Feature flags via `feature()` from `bun:bundle` gate hooks at compile time. Voice-related hooks are the most common example, using conditional `require()` for dead-code elimination.
- AppState (`../state/AppState.js`) is the primary global state store. Use `useAppState(selector)` for reading and `useSetAppState()` for writing. Prefer selectors to avoid unnecessary re-renders.
- The `useTerminalSize()` hook is commonly needed for layout calculations in hooks that produce UI-affecting data.
- Many hooks depend on utilities in `../utils/` for permission logic, message handling, model resolution, and session management.
- The `useInput()` hook from `../ink.js` is used for raw key event handling, but the preferred pattern for keyboard shortcuts is `useKeybinding()` from `../keybindings/useKeybinding.js`.
- External state that lives outside React (e.g., bootstrap state, config files) is typically bridged via `useSyncExternalStore`.

### Common Patterns

- **Permission Hook Pattern**: `useCanUseTool` is the canonical permission gate. It creates a permission context, checks rules/classifier, then either auto-allows or queues an interactive prompt that resolves via a Promise.
- **Polling Pattern**: Hooks like `useSwarmPermissionPoller`, `useInboxPoller`, and `useRemoteSession` use `useInterval` from `usehooks-ts` for periodic polling of external state.
- **Bridge Pattern**: `useReplBridge` establishes a persistent connection and syncs messages bidirectionally. Other hooks consume bridge state via `useAppState`.
- **Typeahead Pattern**: `useTypeahead` manages suggestion state, ranking, and selection. It integrates with `fileSuggestions.ts` and `unifiedSuggestions.ts` for candidate generation.
- **Notification Pattern**: Hooks in `notifs/` use `useNotifications()` from `../context/notifications.js` to push status bar messages. Each notification hook is self-contained with its own display logic and dismissal conditions.
- **Feature-Gated Import Pattern**: `const module = feature('X') ? require('./module.js') : fallback;` ensures unused code is eliminated at compile time. This pattern is used for voice, coordinator, and other optional features.
- **Session/Remote Pattern**: `useRemoteSession`, `useSSHSession`, and `useDirectConnect` share a similar interface (`isRemoteMode`, `sendMessage`, `cancelRequest`, `disconnect`) for polymorphic REPL integration.
