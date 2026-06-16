# Workspace Layout

Workyard uses the repository root for reusable guidance and `.local/` for disposable execution state.

## Tracked Directories

- `agents/`: reusable agent roles and behavior profiles
- `catalog/`: engineering standards and implementation guidance for agents
- `docs/`: project documentation
- `mcp/`: MCP server and profile examples
- `prompts/`: prompt templates
- `runtimes/`: runtime-specific launch and permission profiles
- `tasks/`: task templates and active task notes

## Untracked Local Directories

- `.local/repos/`: cloned target repositories
- `.local/worktrees/`: task-specific worktrees
- `.local/runs/`: run logs and generated execution metadata
- `.local/scratch/`: temporary files

`.local/` must remain ignored by git.
