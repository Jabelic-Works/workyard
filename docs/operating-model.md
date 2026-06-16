# Operating Model

Workyard separates three concerns:

1. The repositories being changed.
2. The agent-readable knowledge used during work.
3. The local execution state for each task.

## Local State

All disposable local state belongs under `.local/`.

```text
.local/
├── repos/
├── worktrees/
├── runs/
└── scratch/
```

This keeps the Workyard repository itself small and reviewable while allowing many target repositories and task worktrees to exist side by side.

## Progressive Workflow

1. Define the task in `tasks/` or a GitHub issue.
2. Identify the target repository or repositories.
3. Create or reuse local clones under `.local/repos/`.
4. Create task-specific worktrees under `.local/worktrees/`.
5. Select relevant catalog entries, prompts, runtime profile, and MCP profile.
6. Run the agent.
7. Verify the target repositories with their own checks.
8. Summarize outputs and next actions.

## Catalog First

Agent behavior should be guided by explicit catalog entries instead of implicit assumptions.

Catalog entries can describe stacks, layers, patterns, and verification checks. Runtime prompts can then assemble the relevant entries for a task.
