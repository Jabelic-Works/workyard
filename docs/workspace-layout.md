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

## Repository Registry

Canonical Repository Referenceは、Repository CloneやTask WorktreeではなくWorkspace Metadataに属する。

ここでいうCanonical Repository Referenceは、Target Repositoryの正とみなすremote referenceを指す。例えば、repository name、canonical URL、provider上のrepository ID、default branchなどを持ち得る。

Local clone path、worktree path、credential value、account-specific remote aliasはCanonical Repository Referenceに混ぜない。それらはLocal Stateまたはaccount-specific configurationとして扱う。

## Open Design

Workspace configuration、workspace metadata、repository registry、plugin install stateの正確な置き場所はまだ固定しない。

初期段階では、credential valueやaccount-specific configurationをpublic repositoryに入れないこと、local stateをtracked fileに混ぜないことを優先する。
