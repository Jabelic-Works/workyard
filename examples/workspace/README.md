# Example User Workspace

このdirectoryは、Workyard User Workspaceの初期形を示す。

これはexampleであり、Workyard Repository rootではない。

## Layout

```text
.
├── agents/      # reusable agent role
├── catalog/     # agent-readable engineering guidance
├── mcp/         # MCP server templateとprofile
├── prompts/     # reusable prompt template
├── runtimes/    # runtime profile
└── tasks/       # task templateとactive task note
```

実際のUser Workspaceでは、disposableなlocal stateを `.local/` に置く。このexampleでは `.local/` をtrackしない。

## Repository Registry

Target RepositoryのCanonical Repository Referenceは、`.local/` ではなくWorkspace Metadataに置く。

正確なfile pathやschemaはまだ固定しない。Local clone path、worktree path、credential value、account-specific remote aliasはCanonical Repository Referenceに混ぜない。
