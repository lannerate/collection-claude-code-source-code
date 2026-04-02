<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# constants

## Purpose
Application-wide constants and configuration values. Centralizes magic numbers, API limits, feature flags, beta identifiers, error codes, UI figures, key bindings, message templates, prompt sections, tool limits, and XML schemas used throughout the codebase.

## Key Files

| File | Description |
|------|-------------|
| `apiLimits.ts` | API rate limits and token limits |
| `betas.ts` | Beta feature identifiers |
| `common.ts` | Shared constants |
| `cyberRiskInstruction.ts` | Cyber risk assessment instructions |
| `errorIds.ts` | Error code identifiers |
| `figures.ts` | Unicode figures and symbols for terminal UI |
| `files.ts` | File-related constants (paths, extensions) |
| `github-app.ts` | GitHub App configuration constants |
| `keys.ts` | Key name constants |
| `messages.ts` | Message template strings |
| `oauth.ts` | OAuth configuration constants |
| `outputStyles.ts` | Output style definitions |
| `product.ts` | Product metadata and version info |
| `prompts.ts` | Prompt-related constants |
| `spinnerVerbs.ts` | Verb list for spinner status messages |
| `system.ts` | System-level constants |
| `systemPromptSections.ts` | System prompt section identifiers |
| `toolLimits.ts` | Tool-specific limits (max file size, etc.) |
| `tools.ts` | Tool name constants and identifiers |
| `turnCompletionVerbs.ts` | Turn completion verb definitions |
| `xml.ts` | XML tag definitions for structured output |

## For AI Agents

### Working In This Directory
- Import from these constants rather than hardcoding values
- `betas.ts` maps to feature-gated functionality controlled by `feature()` checks
- `systemPromptSections.ts` defines the ordered sections of the system prompt
- `toolLimits.ts` is critical for understanding tool behavior constraints
- Changes here affect the entire application — verify downstream impacts
