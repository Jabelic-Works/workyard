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

## Workspace Rootの識別

User Workspaceは、Workspace MetadataによってWorkyard workspaceとして認識される。

Workspace Metadataは、workspace configuration、Repository Registry、enabled plugin referenceなど、workspaceを再現または説明するためのdataを持つ。

Workspace Rootを識別するfile/directoryやschemaはまだ固定しない。User Workspaceの形を示すtracked exampleも、最終的なschemaではない。

## State Boundaries

Workspace MetadataとLocal Stateは分ける。

| Concern | Belongs To | Tracked | Notes |
| --- | --- | --- | --- |
| Workspace configuration | Workspace Metadata | public-safeなら可 | workspace-wideな設定やenabled plugin referenceを表す |
| Repository Registry | Workspace Metadata | public-safeなら可 | Target RepositoryとCanonical Repository Referenceの対応を表す |
| Repository Clone | Local State | 不可 | `.local/repos/` などに置くlocal clone |
| Task Worktree | Local State | 不可 | `.local/worktrees/` などに置くtask-specificなworktree |
| Run | Local State | 不可 | `.local/runs/` などに置く実行logやgenerated metadata |
| Account-Specific Configuration | user-local state | 不可 | user account、team、channel、file、organizationなどに紐づく設定 |
| Credential Value | environment / keychainなど | 不可 | credential valueはpublic repositoryやtracked exampleに置かない |

Credential Referenceはpublic templateに置けるが、credential valueは置かない。

## Repository State Flow

Target Repository、Repository Clone、Task Worktree、Runは同じものではない。

1. Repository Registryに、Target RepositoryとCanonical Repository Referenceの対応を記録する。
2. 必要になったら、Canonical Repository ReferenceからRepository Cloneを `.local/repos/` などに作る。
3. Taskごとに、Repository CloneからTask Worktreeを `.local/worktrees/` などに作る。
4. Agent実行ごとに、Runのlogやgenerated metadataを `.local/runs/` などに置く。

Canonical Repository Referenceには、Git URL、provider上のrepository ID、default branchなどを含められる。Local clone path、worktree path、credential value、account-specific remote aliasは含めない。

## Open Design

次の詳細は、実装PRで必要になった時点で決める。

- Workspace Rootを識別するfile/directoryのpath
- workspace metadataのschema
- config file format
- lockfileの有無と役割
- CLI command
- plugin manifestとの接続

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
