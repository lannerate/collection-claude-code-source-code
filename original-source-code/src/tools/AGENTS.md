<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-04-01 | Updated: 2026-04-01 -->

# tools

## Purpose
Contains 40+ tool implementations that Claude Code can invoke. Each tool is a self-contained module with its own prompt, input schema, and execution logic. Tools are registered via `getTools()` in the parent `tools.ts` using the `buildTool()` factory pattern from `Tool.ts`.

## Key Files

| File | Description |
|------|-------------|
| `utils.ts` | Shared utility functions used across tool implementations |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `AgentTool/` | Sub-agent spawning and management, includes built-in agent definitions |
| `AskUserQuestionTool/` | Interactive user question prompts with option selection |
| `BashTool/` | Shell command execution with sandboxing and output streaming |
| `BriefTool/` | Briefing/context handoff between agent turns |
| `ConfigTool/` | Runtime configuration read/write operations |
| `EnterPlanModeTool/` | Enter plan mode for structured task planning |
| `EnterWorktreeTool/` | Create isolated git worktree for branch work |
| `ExitPlanModeTool/` | Exit plan mode and return to execution |
| `ExitWorktreeTool/` | Exit and optionally remove a git worktree |
| `FileEditTool/` | Precise string replacement edits in existing files |
| `FileReadTool/` | File reading with line ranges and PDF support |
| `FileWriteTool/` | Complete file creation and overwrites |
| `GlobTool/` | Fast file pattern matching using glob syntax |
| `GrepTool/` | Content search using ripgrep with regex support |
| `LSPTool/` | Language Server Protocol integration (go-to-def, references, hover) |
| `ListMcpResourcesTool/` | List available MCP server resources |
| `MCPTool/` | Generic MCP tool call execution |
| `McpAuthTool/` | MCP server authentication handling |
| `NotebookEditTool/` | Jupyter notebook cell editing |
| `PowerShellTool/` | Windows PowerShell command execution |
| `REPLTool/` | Persistent REPL environment for code execution |
| `ReadMcpResourceTool/` | Read specific MCP resource content |
| `RemoteTriggerTool/` | Trigger actions on remote sessions |
| `ScheduleCronTool/` | Cron-based task scheduling |
| `SendMessageTool/` | Inter-agent messaging for team coordination |
| `SkillTool/` | Skill invocation and execution |
| `SleepTool/` | Timed delays for polling/waiting patterns |
| `SyntheticOutputTool/` | Synthetic output generation for testing |
| `TaskCreateTool/` | Create tasks in task list |
| `TaskGetTool/` | Retrieve task details by ID |
| `TaskListTool/` | List all tasks with status summaries |
| `TaskOutputTool/` | Get output from background tasks |
| `TaskStopTool/` | Stop running background tasks |
| `TaskUpdateTool/` | Update task status, owner, dependencies |
| `TeamCreateTool/` | Create coordinated agent teams |
| `TeamDeleteTool/` | Delete teams and cleanup resources |
| `TodoWriteTool/` | Todo list management (legacy task interface) |
| `ToolSearchTool/` | Search available tools by name/capability |
| `WebFetchTool/` | Fetch and parse web content |
| `WebSearchTool/` | Web search using search API |
| `shared/` | Shared code across multiple tools |
| `testing/` | Test utilities and helpers for tool testing |

## For AI Agents

### Working In This Directory
- Each tool follows the same pattern: `prompt` (system prompt), `ToolInputJSONSchema` (input validation), `buildTool()` (factory function)
- Tools are registered in parent `tools.ts` via `getTools()`, not self-registered
- The `AgentTool/built-in/` subdirectory contains predefined agent configurations
- `shared/` contains common utilities — check here before duplicating logic

### Common Patterns
- `buildTool()` factory returns tool config with name, description, schema, and execute function
- Input validation uses JSON Schema via `ToolInputJSONSchema`
- Tools yield streaming results via async generators
- Permission checks delegated to `useCanUseTool` hook
- File tools use atomic operations to prevent corruption
