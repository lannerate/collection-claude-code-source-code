<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# keybindings

## Purpose
Keyboard shortcut system for the CLI. Defines default keybindings, parses user-customized bindings, resolves key sequences to actions, and provides the KeybindingProvider React context. Supports vim-style modal keybindings and multi-key sequences.

## Key Files

| File | Description |
|------|-------------|
| `KeybindingContext.tsx` | React context for keybinding state |
| `KeybindingProviderSetup.tsx` | Provider initialization and setup |
| `defaultBindings.ts` | Default keyboard shortcut definitions |
| `loadUserBindings.ts` | Load user-customized keybindings from config |
| `match.ts` | Match key events to bound actions |
| `parser.ts` | Parse keybinding notation (e.g., "ctrl+a", "g g") |
| `reservedShortcuts.ts` | Shortcuts reserved by the terminal/OS |
| `resolver.ts` | Resolve keybinding conflicts and priorities |
| `schema.ts` | JSON schema for keybinding configuration |
| `shortcutFormat.ts` | Shortcut display formatting |
| `template.ts` | Keybinding template utilities |
| `useKeybinding.ts` | React hook for consuming keybindings |
| `useShortcutDisplay.ts` | Hook for displaying shortcut help |
| `validate.ts` | Validate keybinding configurations |

## For AI Agents

### Working In This Directory
- `defaultBindings.ts` is the source of truth for all default shortcuts
- `parser.ts` converts keybinding notation strings to matcher functions
- The system supports multi-key sequences (vim-style) via `resolver.ts`
- `reservedShortcuts.ts` prevents conflicts with terminal/OS shortcuts
- User customizations in `loadUserBindings.ts` override defaults
