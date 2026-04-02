<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# outputStyles

## Purpose
Output formatting style definitions. Controls how different types of content (tool results, errors, info messages) are rendered in the terminal — colors, formatting, and layout for each output style mode.

## Key Files

| File | Description |
|------|-------------|
| `outputStyles.ts` | Output style registry and formatting definitions |

## For AI Agents

### Working In This Directory
- Single-file module defining all output style variants
- Styles are referenced by name throughout `components/messages/`
- The `/output-style` command in `commands/output-style/` switches between styles
