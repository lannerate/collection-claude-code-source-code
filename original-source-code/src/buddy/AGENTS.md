<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# buddy

## Purpose

Implements the "Buddy" lightweight companion system -- a virtual pet that sits beside the user's input box and occasionally comments in a speech bubble. Each user gets a deterministically-generated companion with randomized species, rarity, stats, and visual appearance. The companion hatches once and persists via global config, providing a playful ambient presence during CLI sessions. Feature-gated behind the `BUDDY` feature flag.

## Key Files

| File | Description |
|------|-------------|
| `companion.ts` | Deterministic companion generation via seeded PRNG (`mulberry32`). `roll()` produces `CompanionBones` (species, rarity, eye, hat, shiny, stats) from a hashed user ID. `getCompanion()` merges stored soul data with regenerated bones. |
| `prompt.ts` | Generates the companion intro text and attachment that teaches the main agent how to coexist with the companion. Injects a `companion_intro` attachment when a companion is active and not muted. |
| `types.ts` | Core type definitions: `Companion`, `CompanionBones`, `CompanionSoul`, `StoredCompanion`. Defines 18 species (duck, goose, dragon, octopus, capybara, etc.), 6 eye styles, 8 hat types, 5 rarities with weighted probabilities, and 5 stat names. Species names are runtime-constructed via `String.fromCharCode` to avoid build-time canary detection. |
| `sprites.ts` | ASCII art sprite definitions for each species. Multiple animation frames per species (3 frames each, 5 lines tall, 12 chars wide). Hat slot on line 0, eye placeholder `{E}` for dynamic eye rendering. |
| `CompanionSprite.tsx` | React/Ink component that renders the companion sprite in the terminal UI. Handles animation timing, eye blink, and hat rendering. The largest file in the module (~46KB) due to sprite frame data and rendering logic. |
| `useBuddyNotification.tsx` | React hook that manages the buddy notification lifecycle: shows a rainbow teaser during the April 1-7, 2026 window, and displays companion presence during normal operation. Uses the notifications context system. |

## Subdirectories

None.

## For AI Agents

### Working In This Directory

- The entire system is feature-gated behind `feature('BUDDY')`. Check this flag before modifying behavior.
- Species names in `types.ts` are deliberately obfuscated via `String.fromCharCode` to avoid colliding with a model-codename canary in the build system's excluded-strings check. Do not replace them with string literals.
- `companion.ts` caches the roll result per user ID for performance (called from hot paths: 500ms sprite tick, per-keystroke input, per-turn observer).
- Bones are never persisted -- they are regenerated from the user ID hash on every read. Only `CompanionSoul` (name, personality) and `hatchedAt` are stored in config. This prevents users from editing their way to a legendary rarity.
- Rarity weights: common (60), uncommon (25), rare (10), epic (4), legendary (1).
- The teaser window is date-based (April 1-7, 2026 local time) for a rolling timezone wave rather than a single UTC spike.

### Common Patterns

- **Seeded deterministic generation**: `mulberry32` PRNG seeded with `hashString(userId + SALT)` ensures the same user always gets the same companion.
- **Soul/bones split**: Immutable visual traits (bones) regenerated from hash; personality traits (soul) persisted after first hatch.
- **React/Ink rendering**: Companion sprite uses Ink's `<Text>` component with color props for terminal rendering.
