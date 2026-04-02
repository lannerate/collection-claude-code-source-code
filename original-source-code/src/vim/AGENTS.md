<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# vim

## Purpose

Implements a complete vim mode state machine for Claude Code's input editor. Provides vim-like modal editing with INSERT and NORMAL modes, supporting operators (delete, change, yank), motions (word/line/character navigation), text objects (inner/around word, quotes, brackets), find commands (f/F/t/T), and dot-repeat. The system is built as a pure functional state machine with no side effects in the core logic.

## Key Files

| File | Description |
|------|-------------|
| `types.ts` | Complete state machine type definitions. `VimState` (INSERT/NORMAL modes), `CommandState` (10 states: idle, count, operator, operatorCount, operatorFind, operatorTextObj, find, g, operatorG, replace, indent), `PersistentState` (register, lastFind, lastChange for dot-repeat), `RecordedChange` (9 change types). Key group constants and factory functions. |
| `motions.ts` | Pure motion resolution functions. `resolveMotion()` applies a motion key with count repetition. `isInclusiveMotion()` and `isLinewiseMotion()` classify motions for operator context. Supports h/l/j/k/w/b/e/W/B/E/0/^/$/G/gg/gj/gk. |
| `operators.ts` | Operator execution functions (delete, change, yank). `OperatorContext` type provides the editor interaction surface. `executeOperatorMotion()`, `executeOperatorTextObj()`, `executeOperatorFind()` and helpers for paste, replace, indent, join, toggle case. |
| `textObjects.ts` | Text object boundary detection. `findTextObject()` locates inner/around ranges for word (w/W), quotes ("/'/`), and bracket pairs (paren, square, curly, angle). Returns `TextObjectRange` with start/end offsets. |
| `transitions.ts` | State transition table -- the scannable source of truth for all state machine transitions. Maps each `CommandState` type to its valid next inputs and resulting states. `TransitionContext` extends `OperatorContext` with undo/dot-repeat callbacks. |

## Subdirectories

None.

## For AI Agents

### Working In This Directory

- The architecture follows a strict separation: `types.ts` defines states, `transitions.ts` maps inputs to state changes, `motions.ts`/`operators.ts`/`textObjects.ts` execute the actual editing operations.
- `types.ts` contains a comprehensive state diagram in its doc comment that explains the full state machine flow. Read it first to understand the system.
- All motion and operator functions are pure -- they take current state and return new state without mutation. The `OperatorContext` callback pattern (`setText`, `setOffset`, `setRegister`, etc.) bridges to the actual editor.
- `MAX_VIM_COUNT` is 10,000 -- count values are bounded to prevent excessive operations.
- The `RecordedChange` union type captures everything needed for dot-repeat replay, including the change type, parameters, and count.
- `SIMPLE_MOTIONS`, `FIND_KEYS`, `TEXT_OBJ_SCOPES`, and `TEXT_OBJ_TYPES` are named constant sets for exhaustive matching.

### Common Patterns

- **Pure functional state machine**: No side effects in core logic; all mutations via callback context.
- **Discriminated union states**: `CommandState` has 10 variants, each with its own data shape. TypeScript enforces exhaustive handling.
- **Operator-motion composition**: Operators compose with motions and text objects following vim's grammar (operator + motion = action).
- **Persistent state**: Register, last find, and last change survive across commands for repeat/paste operations.
