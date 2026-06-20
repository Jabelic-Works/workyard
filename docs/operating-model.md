# Operating Model

Workyardは3つの関心を分ける。

1. 変更対象のrepository。
2. 作業中にagentが読むknowledge。
3. taskごとに生成されるlocal execution state。

## Local State

User Workspaceでは、disposableなlocal stateを `.local/` に置く。

```text
.local/
├── repos/
├── worktrees/
├── runs/
└── scratch/
```

これにより、多数のtarget repositoryやtask worktreeを並べても、Workyard RepositoryやUser Workspaceのtracked fileにlocal stateを混ぜずに済む。

## Progressive Workflow

1. GitHub issue、local note、user requestなどからtaskを決める。
2. target repositoryを特定する。
3. `.local/repos/` にrepository cloneを作る、または再利用する。
4. `.local/worktrees/` にtask worktreeを作る。
5. 必要なcatalog、prompt、runtime profile、MCP profile、pluginを選ぶ。
6. agentを実行する。
7. target repository自身のcheckで検証する。
8. 結果と次のactionをまとめる。

## Catalog First

Agent behaviorは、散らばったcontextから推測させるのではなく、明示的なcatalog entryで導く。

Catalog entryは、stack、layer、pattern、verification checkなどを説明できる。Runtime promptやpluginは、taskに必要なentryを組み合わせる。

## Repository Boundary

Workyard RepositoryはUser Workspaceではない。

Workyard Repository rootには、source、docs、examples、development utilityを置く。User Workspaceの形を示す場合は `examples/workspace/` に置く。
