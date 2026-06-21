# User Workspace Layout

この文書は、利用者がWorkyardをinstallした後に作るUser Workspaceの初期layout案を説明する。

Workyard Repository rootのlayoutではない。Tracked exampleは `examples/workspace/` を参照する。

## Tracked Directories

- `agents/`: reusable agent roleとbehavior profile
- `catalog/`: agent向けengineering standardとimplementation guidance
- `docs/`: workspace-localなdocumentation
- `mcp/`: MCP server templateとprofile example
- `prompts/`: prompt template
- `runtimes/`: runtime-specificなlaunch / permission profile
- `tasks/`: task templateとactive task note

## Untracked Local Directories

- `.local/repos/`: target repositoryのclone
- `.local/worktrees/`: task-specificなworktree
- `.local/runs/`: run logとgenerated execution metadata
- `.local/scratch/`: temporary file

`.local/` はgitでignoreする。

## `.local/` Directory Roles

`repos/` は、Target Repositoryのlocal cloneを置く場所。

Repository Cloneは、Canonical Repository Referenceから取得したworking copy。複数のTask Worktreeを作るための元にもなる。Repository CloneそのものはLocal Stateなので、Workyard RepositoryやUser Workspaceのtracked fileには含めない。

`worktrees/` は、taskごとの編集場所を置く場所。

Task Worktreeは、特定のTaskのためにRepository Cloneから作るGit worktree。同じTarget Repositoryに対する並行作業や、multi-repository taskでのrepositoryごとの編集を分離するために使う。Task Worktree内の変更はtarget repository側でcommit、test、pushするものであり、Workyard Repositoryの変更ではない。

`runs/` は、agent実行ごとの記録を置く場所。

Runは、Taskに対する1回の実行試行。runtime名、試行番号、log、generated metadata、verification noteなどを置ける。Runは作業の履歴や再現補助には使えるが、長く残すべき決定事項を置く場所ではない。

`scratch/` は、一時的な調査fileや変換途中のfileを置く場所。

Scratch fileは、Task完了時に捨てられる前提で扱う。残す価値がある内容は、Workspace Metadata、GitHub issue、PR description、またはtarget repository側のdocumentationに移す。

## Cleanup Policy

`.local/` は使うほど増えるため、定期的に削除できる前提で扱う。

- `runs/`: 古いagent実行logやgenerated metadataは、必要な要約や決定事項を外へ移した後に削除できる。
- `scratch/`: Task完了後に削除できる。
- `worktrees/`: Taskがmergeされた、closeされた、または中止されたら削除できる。未commit変更が残っていないかだけ確認する。
- `repos/`: Canonical Repository Referenceから再取得できるため削除できる。ただし再clone costが大きい場合はcacheとして残してよい。

長く残すべき情報を `.local/` にだけ置かない。Workyardは将来、削除を支援するcommandやdiagnosticsを提供し得るが、この文書では具体的なCLIや保存期間を固定しない。

## `.local/` Path Examples

`.local/` 配下は、User Workspaceのmachine-localでdisposableな作業状態を置く場所。

```text
.local/
├── repos/
│   ├── web-app/
│   └── api-service/
├── worktrees/
│   ├── task-20260621-fix-login-web-app/
│   └── task-20260621-fix-login-api-service/
├── runs/
│   └── task-20260621-fix-login/
│       ├── codex-001/
│       └── claude-001/
└── scratch/
    └── task-20260621-fix-login/
```

名前は、taskが分かる短いslugを含める。日付や連番は衝突を避けるために使えるが、正確な命名規則は実装しながら決める。

## Task Examples

Single-repository taskでは、1つのRepository Cloneから1つのTask Worktreeを作る。

```text
.local/repos/web-app/
.local/worktrees/task-20260621-update-signup-web-app/
.local/runs/task-20260621-update-signup/codex-001/
```

Multi-repository taskでは、同じtask slugの下でrepositoryごとにTask Worktreeを分ける。

```text
.local/repos/web-app/
.local/repos/api-service/
.local/worktrees/task-20260621-update-signup-web-app/
.local/worktrees/task-20260621-update-signup-api-service/
.local/runs/task-20260621-update-signup/codex-001/
```

`.local/` 配下のdataは再生成できる前提で扱う。必要な知識や決定事項を残す場合は、Local StateではなくWorkspace Metadata、GitHub issue、PR description、またはtarget repository側のdocumentationに移す。

## Repository Registry

Canonical Repository Referenceは、Repository CloneやTask WorktreeではなくWorkspace Metadataに属する。

ここでいうCanonical Repository Referenceは、Target Repositoryの正とみなすremote referenceを指す。例えば、repository name、canonical URL、provider上のrepository ID、default branchなどを持ち得る。

Local clone path、worktree path、credential value、account-specific remote aliasはCanonical Repository Referenceに混ぜない。それらはLocal Stateまたはaccount-specific configurationとして扱う。

## Open Design

Workspace configuration、workspace metadata、repository registry、plugin install stateの正確な置き場所はまだ固定しない。

初期段階では、credential valueやaccount-specific configurationをpublic repositoryに入れないこと、local stateをtracked fileに混ぜないことを優先する。
