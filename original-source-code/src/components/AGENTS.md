<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# components/

## Purpose

React/Ink terminal UI components that render the entire Claude Code CLI interface. This directory contains all visual elements -- message displays, dialogs, prompts, spinners, permission requests, settings panels, diff views, and the main layout shell. Components use Ink (a React renderer for the terminal) and are compiled with the React Compiler (`react/compiler-runtime`). The architecture is a deeply nested component tree rooted at `App.tsx` providing context providers, `FullscreenLayout.tsx` managing scrollable message areas and bottom-anchored input, and dozens of specialized subcomponents for each UI concern.

## Key Files

| File | Description |
|------|-------------|
| `App.tsx` | Top-level wrapper; provides FPS metrics, stats, and AppState context to the entire component tree |
| `FullscreenLayout.tsx` | Main layout shell; manages scrollable message area, bottom-anchored prompt, overlay/modal layers, and scroll chrome context |
| `Messages.tsx` | Orchestrates rendering of the full message transcript; normalizes messages, applies grouping (tool use, read/search collapse), and renders MessageRow instances |
| `Message.tsx` | Renders a single message (user text, assistant text, tool use, thinking, system, attachment); dispatches to specialized message subcomponents |
| `MessageRow.tsx` | Row wrapper for each message in the transcript; handles selection, actions nav, and memoization boundaries |
| `VirtualMessageList.tsx` | Virtual-scroll container for the message list; manages scroll position, sticky headers, search highlighting, and jump-to-message navigation |
| `messageActions.tsx` | Context providers for message action state (selection, navigation, copy) shared between MessageRow and VirtualMessageList |
| `TextInput.tsx` | Terminal text input with voice recording waveform cursor, accessibility support, and theme integration |
| `BaseTextInput.tsx` | Low-level Ink-based text input component handling cursor movement, selection, and rendering |
| `VimTextInput.tsx` | Vim-mode text input wrapper providing modal editing (insert/normal/visual) over BaseTextInput |
| `Spinner.tsx` | Activity spinner shown during model inference; displays glimmer animations, teammate tree, task lists, and effort indicators |
| `StatusLine.tsx` | Customizable status line (model, permission mode, context usage, cwd); executes user-defined shell commands for status content |
| `Stats.tsx` | Session statistics display (cost, tokens, duration, lines added/removed) |
| `Markdown.tsx` | Markdown renderer for assistant messages; uses `marked` lexer with token caching for performance |
| `MarkdownTable.tsx` | Table renderer for markdown tables with column alignment and width calculation |
| `Feedback.tsx` | Feedback/bug report dialog; collects description, attaches transcripts, and submits GitHub issues via API |
| `ModelPicker.tsx` | Model selection dialog (Sonnet, Opus, Haiku, custom); includes effort level picker |
| `ThemePicker.tsx` | Theme customization dialog with color scheme selection |
| `OutputStylePicker.tsx` | Output style selection (verbose, concise, etc.) |
| `Onboarding.tsx` | First-run onboarding wizard for new users |
| `AutoUpdater.tsx` | Auto-update checker and installer dialog |
| `ConsoleOAuthFlow.tsx` | OAuth authentication flow for console/terminal sessions |
| `ContextVisualization.tsx` | Context window usage visualization showing token distribution |
| `LogSelector.tsx` | Log file viewer/selector for debugging (200KB) |
| `MessageSelector.tsx` | Message selection and annotation UI (115KB) |
| `ScrollKeybindingHandler.tsx` | Keyboard-driven scroll navigation (j/k, G, gg, search) for the message transcript |
| `StructuredDiff.tsx` | Structured diff view for file edits with syntax highlighting |
| `ContextSuggestions.tsx` | Context-aware input suggestions based on current state |
| `GlobalSearchDialog.tsx` | Global search across sessions and transcripts |
| `HistorySearchDialog.tsx` | Searchable history browser for past commands |
| `QuickOpenDialog.tsx` | Quick-open dialog for files and commands |
| `CompactSummary.tsx` | Summary shown after context compaction |
| `TokenWarning.tsx` | Warning display when approaching token limits |
| `EffortCallout.tsx` | Display for extended thinking/effort level indicators |
| `ThinkingToggle.tsx` | Toggle for extended thinking mode |
| `CoordinatorAgentStatus.tsx` | Status display for coordinator/multi-agent mode |
| `SentryErrorBoundary.ts` | Error boundary that reports unhandled errors to Sentry |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `agents/` | Agent creation and management UI -- agent list, editor, detail view, color/model/tool selectors, and a multi-step creation wizard (`new-agent-creation/wizard-steps/`) |
| `CustomSelect/` | Reusable custom select component with single/multi-select, navigation, and input filtering |
| `design-system/` | Shared UI primitives: Dialog, Pane, Divider, Tabs, ListItem, ProgressBar, Ratchet, FuzzyPicker, KeyboardShortcutHint, Byline, ThemedText/Box, ThemeProvider, color utilities, LoadingState, StatusIcon |
| `diff/` | Diff viewing components: DiffDialog, DiffFileList, DiffDetailView for comparing file changes |
| `FeedbackSurvey/` | Post-session feedback survey, transcript sharing, memory survey, debounced input |
| `grove/` | Grove visualization component |
| `HelpV2/` | Help system with commands reference and general help views |
| `hooks/` | Hook configuration UI -- select event mode, hook mode, matcher, and view hook configuration |
| `hooks/` (component-local) | Hook configuration menu components for the settings UI |
| `LogoV2/` | Animated logo, welcome screen, channel notices, upsell prompts, emergency tips, feed columns |
| `mcp/` | MCP server management UI -- settings, list panel, stdio/remote/agent server menus, tool detail/list views, reconnect, capabilities section, elicitation dialog, parsing warnings |
| `memory/` | Memory file selector and memory update notification components |
| `messages/` | Individual message type renderers (~30 types): AssistantTextMessage, AssistantThinkingMessage, AssistantToolUseMessage, UserTextMessage, UserToolResultMessage (with sub-variants: success, error, reject, canceled), SystemTextMessage, CompactBoundaryMessage, HookProgressMessage, PlanApprovalMessage, RateLimitMessage, TaskAssignmentMessage, and more |
| `permissions/` | Permission request UI for each tool type (Bash, FileEdit, FileWrite, Filesystem, NotebookEdit, PowerShell, WebFetch, Skill, AskUserQuestion, ComputerUse, EnterPlanMode, ExitPlanMode); includes permission rule management, workspace directory management, and shell permission helpers |
| `PromptInput/` | Prompt input assembly: main input, footer, suggestions, history search, notifications, mode indicator, queued commands, sandbox hints, voice indicator, stash notice, fast icon hint |
| `sandbox/` | Sandbox settings UI: config tab, dependencies tab, overrides tab, doctor section |
| `Settings/` | Settings panels: main settings view, config editor, status display, usage tracking |
| `shell/` | Shell output components: output line rendering, progress messages, time display, expand/collapse context |
| `skills/` | Skills management menu |
| `Spinner/` | Spinner subsystem: animated glyphs, shimmer/stalled animations, teammate spinner tree, glimmer messages |
| `StructuredDiff/` | Structured diff fallback renderer and color diff utilities |
| `tasks/` | Task detail dialogs: background tasks, async agents, in-process teammates, remote sessions, shell progress, dream tasks |
| `teams/` | Team status display and teams management dialog |
| `TrustDialog/` | Trust dialog for new project directories with trust acceptance logic |
| `ui/` | Generic UI utilities: OrderedList, OrderedListItem, TreeSelect |
| `wizard/` | Reusable wizard framework: WizardProvider, WizardNavigationFooter, WizardDialogLayout, useWizard hook |
| `HighlightedCode/` | Highlighted code fallback renderer |
| `LspRecommendation/` | LSP recommendation menu |
| `Passes/` | Passes/credits display |
| `DesktopUpsell/` | Desktop app upsell prompt at startup |
| `ManagedSettingsSecurityDialog/` | Managed settings security configuration dialog |

## For AI Agents

### Working In This Directory

- All components use React with Ink (terminal UI framework). The `Box`, `Text`, and `Ansi` primitives from `../ink.js` are the standard building blocks.
- The code is decompiled from a compiled bundle and uses the React Compiler runtime (`_c` from `react/compiler-runtime`). Memoization hooks like `$ = _c(N)` are compiler-generated cache slots -- do not modify them.
- Feature flags via `feature()` from `bun:bundle` gate entire component subtrees. Conditional imports use `require()` for feature-gated modules.
- Theme colors come from `useTheme()` and `Theme` type in `src/utils/theme.ts`. Use `ThemedText` and `ThemedBox` from `design-system/` for themed rendering.
- Keybinding registration uses `useKeybinding()` and `useKeybindings()` from `../keybindings/useKeybinding.js`. New keyboard interactions should follow this pattern.
- AppState (`../state/AppState.js`) provides global state via `useAppState` and `useSetAppState`. Prefer local component state for UI concerns; use AppState for cross-cutting state.
- The `useTerminalSize()` hook from `../hooks/useTerminalSize.js` is used pervasively for responsive layout.
- Components are organized by feature domain, not by technical type. When adding a new UI element, place it in the appropriate feature subdirectory or alongside related components.

### Common Patterns

- **Component Props**: Props are defined as inline `type Props = { ... }` at the top of each file. No separate prop files.
- **Dialog Pattern**: Most dialogs use the `Dialog` or `Pane` component from `design-system/`, which provides border rendering, title, cancel handling, and keyboard shortcuts.
- **Permission Request Pattern**: Each tool type has its own permission request component in `permissions/` that receives a `ToolUseConfirm` object and resolves with a `PermissionDecision`.
- **Message Rendering Pattern**: `Message.tsx` dispatches to specialized subcomponents in `messages/` based on message type. Each message component is self-contained with its own rendering logic.
- **Wizard Pattern**: Multi-step flows use the `wizard/` framework with `WizardProvider`, step components, and `useWizard` hook for navigation state.
- **Select Pattern**: `CustomSelect/` provides single and multi-select components used throughout for user choices.
- **Notification Pattern**: Status bar notifications use `useNotifications()` from `../context/notifications.js`.
- **Error Boundary**: Wrap risky subtrees with `SentryErrorBoundary` for crash reporting.
