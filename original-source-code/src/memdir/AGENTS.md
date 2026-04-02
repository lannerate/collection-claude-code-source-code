<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# memdir

## Purpose
Memory directory management for session persistence and team memory. Handles scanning, reading, and aging of memory files stored in the `.claude/` directory. Supports both user-level memories and team-level memories with relevance-based retrieval.

## Key Files

| File | Description |
|------|-------------|
| `memdir.ts` | Main memory directory management — scan, read, organize |
| `memoryScan.ts` | Scan memory directories for relevant files |
| `memoryTypes.ts` | Type definitions for memory entries |
| `memoryAge.ts` | Memory entry aging and staleness detection |
| `findRelevantMemories.ts` | Relevance-based memory retrieval |
| `paths.ts` | Path utilities for memory file locations |
| `teamMemPaths.ts` | Team memory path resolution |
| `teamMemPrompts.ts` | Team memory prompt generation |

## For AI Agents

### Working In This Directory
- Memory files live under `.claude/` in project or home directories
- `findRelevantMemories.ts` is the main entry point for memory retrieval
- `memoryAge.ts` determines when memories are stale (default: 7 days)
- Team memories use separate paths managed by `teamMemPaths.ts`
- `teamMemPrompts.ts` generates system prompt sections from team memories
