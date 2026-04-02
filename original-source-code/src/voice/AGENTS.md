<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# voice

## Purpose

Implements voice mode enablement checks for Claude Code. Provides a layered gating system that controls whether voice input/output features are available. Voice mode requires both Anthropic OAuth authentication (it uses the `voice_stream` endpoint on claude.ai) and a GrowthBook kill-switch check. The module separates auth checks from feature flag checks so that React render paths can memoize the expensive keychain read while command paths get a fresh check.

## Key Files

| File | Description |
|------|-------------|
| `voiceModeEnabled.ts` | Three exported functions: `isVoiceGrowthBookEnabled()` (feature flag + GrowthBook kill-switch `tengu_amber_quartz_disabled`), `hasVoiceAuth()` (Anthropic OAuth token check via keychain), and `isVoiceModeEnabled()` (combined auth + flag check). Feature-gated behind `VOICE_MODE` compile-time flag. |

## Subdirectories

None.

## For AI Agents

### Working In This Directory

- This is a single-file module. All functionality lives in `voiceModeEnabled.ts`.
- Voice mode is triple-gated: (1) compile-time `feature('VOICE_MODE')`, (2) runtime GrowthBook kill-switch, (3) Anthropic OAuth token presence.
- `isVoiceGrowthBookEnabled()` uses a positive ternary pattern to avoid eliminating inline string literals from external builds. The GrowthBook flag is negated (`!getFeatureValue_CACHED_MAY_BE_STALE('tengu_amber_quartz_disabled', false)`) because the default `false` means "not killed."
- `hasVoiceAuth()` is backed by a memoized `getClaudeAIOAuthTokens()` -- first call spawns the macOS `security` binary (~20-50ms), subsequent calls are cache hits. Cache clears on token refresh (~once/hour).
- Voice mode is NOT available with API keys, Bedrock, Vertex, or Foundry auth -- it requires Anthropic OAuth because it calls claude.ai's `voice_stream` endpoint.
- For React render paths, use `useVoiceEnabled()` (defined elsewhere) which memoizes the auth half. `isVoiceModeEnabled()` is for command-time paths where a fresh keychain read is acceptable.

### Common Patterns

- **Layered gating**: Compile-time feature flag -> runtime feature flag -> auth check, each layer independent.
- **Positive ternary pattern**: `return feature('X') ? check() : false` avoids negative early returns that eliminate inline strings.
- **Memoized auth check**: Keychain reads are expensive; memoize in render paths, fresh read in command paths.
