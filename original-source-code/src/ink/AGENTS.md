<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# ink

## Purpose
Extended Ink rendering engine — a customized fork of the Ink terminal UI framework. Provides the core rendering pipeline: React reconciler, terminal output, text wrapping, layout engine, focus management, keypress parsing, and terminal querying. 43+ files plus subdirectories for hooks, components, events, layout, and termio.

## Key Files

| File | Description |
|------|-------------|
| `ink.tsx` | Main Ink entry point and provider |
| `reconciler.ts` | Custom React reconciler for terminal output |
| `renderer.ts` | Render pipeline coordinator |
| `render-to-screen.ts` | Screen rendering output |
| `render-node-to-output.ts` | Node-to-terminal-output conversion |
| `render-border.ts` | Border rendering for boxes |
| `terminal.ts` | Terminal abstraction and control |
| `termio.ts` | Terminal I/O management |
| `screen.ts` | Screen buffer and diffing |
| `output.ts` | Output stream management |
| `focus.ts` | Focus management system |
| `events/` | Event handling subsystem |
| `hooks/` | Ink-specific React hooks |
| `components/` | Base Ink components (Box, Text, etc.) |
| `layout/` | Layout engine (flexbox-like) |
| `termio/` | Terminal I/O submodules |
| `styles.ts` | Style processing and application |
| `wrap-text.ts` | Text wrapping with terminal awareness |
| `wrapAnsi.ts` | ANSI escape code-aware wrapping |
| `stringWidth.ts` | Terminal-aware string width calculation |
| `measure-text.ts` | Text measurement for layout |
| `measure-element.ts` | Element measurement |
| `parse-keypress.ts` | Keypress event parsing |
| `selection.ts` | Text selection support |
| `searchHighlight.ts` | Search result highlighting |
| `Ansi.tsx` | ANSI output component |
| `bidi.ts` | Bidirectional text support |
| `optimizer.ts` | Output optimization (minimize redraws) |
| `dom.ts` | Virtual DOM utilities |
| `instances.ts` | Ink instance management |
| `root.ts` | Root component |
| `log-update.ts` | Log line update utility |
| `squash-text-nodes.ts` | Text node consolidation |
| `clearTerminal.ts` | Terminal clearing |
| `colorize.ts` | Color application |
| `constants.ts` | Shared constants |
| `node-cache.ts` | Node caching for performance |
| `line-width-cache.ts` | Line width caching |
| `widest-line.ts` | Widest line calculation |
| `get-max-width.ts` | Max width calculation |
| `hit-test.ts` | Hit testing for elements |
| `frame.ts` | Frame rendering |
| `tabstops.ts` | Tab stop handling |
| `terminal-focus-state.ts` | Terminal focus tracking |
| `terminal-querier.ts` | Terminal capability querying |
| `supports-hyperlinks.ts` | Hyperlink support detection |
| `useTerminalNotification.ts` | Terminal notification hook |
| `warn.ts` | Warning utilities |

## For AI Agents

### Working In This Directory
- This is a customized Ink fork — not a standard Ink installation
- The reconciler (`reconciler.ts`) bridges React's reconciliation to terminal output
- Layout follows a flexbox-like model in `layout/`
- Text handling is Unicode-aware via `stringWidth.ts` and `wrap-text.ts`
- Performance-critical paths use caching (`node-cache.ts`, `line-width-cache.ts`, `optimizer.ts`)

### Common Patterns
- React components render to virtual terminal buffer, not DOM
- `measure-text.ts` and `stringWidth.ts` handle CJK and emoji width correctly
- `parse-keypress.ts` converts raw terminal input to key events
