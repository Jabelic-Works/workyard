# ユビキタス言語

Workyardの設計で使う言葉を定義する。

最初の目的は、次の3つを混ぜないこと。

- Workyard自体を開発するrepository
- 利用者がWorkyardをinstallした後の作業場
- pluginやexternal toolでWorkyardを拡張する仕組み

## Core Terms

### Workyard

productとecosystem全体を指す言葉。

より具体的に言える場合は、Workyard Repository、Workyard CLI、User Workspace、Pluginなどの用語を使う。

### Workyard Repository

Workyard自体を開発するsource repository。

source code、documentation、example、test、first-party package sourceを置く。実利用者の作業directoryの形をそのままrootに置くものではない。

### Workyard Core

Workyardを使えて拡張できる状態にするための最小機能。

workspace initialization、configuration resolution、plugin discovery、plugin lifecycle、capability inspection、diagnosticsなどを扱う。domain-specific featureは原則coreの外に置く。

### Workyard CLI

利用者や開発者が実行するcommand line interface。

Workyard Coreとinstalled pluginを使うためのdelivery surfaceであり、Workyard全体そのものではない。

## Workspace Terms

### User Workspace

利用者が作る、またはWorkyardが管理するlocal workspace。

target repository、task worktree、run、installed capabilityなどを使って作業を進める場所。

### Workspace Root

User Workspaceのfilesystem root。

`workyard init`などで作られる可能性があるdirectory。Workyard Repository rootとは別物。

### Workspace Metadata

User Workspace内にあるWorkyard管理のmetadata。

workspace configuration、enabled plugin reference、local state indexなどが含まれ得る。正確なlayoutは実装しながら決める。

### Repository Registry

Target RepositoryとCanonical Repository Referenceの対応を持つWorkspace Metadata。

どのrepositoryを作業対象にするか、その正とみなすremote referenceは何かを表す。正確なfile pathやschemaは実装しながら決める。

### Local State

Workyard利用中にmachine localに生成されるdata。

repository clone、task worktree、run log、cache、generated config、scratch fileなどを含む。publicなWorkyard Repositoryにはcommitしない。

### Example Workspace

User Workspaceがどのような形になるかを示すtracked example。

Workyard Repository内にworkspace風のdirectoryを置く場合は、原則`examples/`配下に置き、exampleであることを明示する。

## Repository Work Terms

### Target Repository

Workyardを通じて調査、編集、test、coordinationしたいrepository。

Workyardがlocal作業を支援しても、そのrepositoryのsource codeをWorkyardが所有するわけではない。

### Canonical Repository Reference

Target Repositoryの正とみなすremote repositoryへの参照。

Git URL、provider上のrepository ID、default branchなどを含み得る。credential value、local clone path、account-specific remote aliasは含めない。

### Repository Clone

Target Repositoryのlocal clone。

Repository CloneはLocal State。User Workspaceが管理することはあるが、Workyard Repositoryにvendorしない。

### Task Worktree

Target Repositoryから作るtask-specificなGit worktree。

Task Worktreeは、関係ない変更を混ぜずに並行作業するために使う。

### Task

goalとexpected outcomeを持つ、境界づけられた作業単位。

TaskはGitHub issue、local note、user requestなどから始まる。TaskとGitHub issueは同義ではない。

### Run

Taskに対する1回の実行試行。

Runには、selected plugin、runtime profile、prompt、log、result、verification noteなどが含まれ得る。

## Knowledge Terms

### Catalog

agentが参照できるengineering guidance。

standard、pattern、check、decisionなどを説明する。生成されたcodeやruntime configurationとは別物。

### Catalog Pack

pluginまたはbundleが提供するCatalog entryのまとまり。

### Prompt

agentやuser workflowを導くための再利用可能なinstruction text。

### Prompt Pack

pluginまたはbundleが提供するPromptのまとまり。

### Agent Role

planner、implementer、reviewerなど、agentに与える再利用可能な役割定義。

Agent Roleはresponsibilityとboundaryを説明する。Runtimeとは別物。

## Runtime Terms

### Runtime

Workyardと一緒に使うAI coding agentまたはexecution surface。

Codex、Claude、Geminiなどが例になり得る。Workyardは、全利用者が全Runtimeを持っているとは仮定しない。

### Runtime Profile

Runtimeの起動方法や利用方法に関するWorkyard側のguidance。

permission behavior、workspace access、environment assumption、verification stepなどを説明できる。

### Skill

Runtimeがskill conceptを持つ場合の、runtime-specificな再利用可能capability。

SkillをPluginの一般的な同義語として使わない。

### Skill Pack

Runtimeが対応する場合にinstall、reference、adaptできるSkillのまとまり。

## Plugin Terms

### Plugin

Workyardにcapabilityを追加するextension。

Catalog entry、Prompt、Runtime Profile、MCP integration guidance、External Tool integration、workflow command、diagnosticsなどを提供できる。

### Plugin Manifest

Pluginを説明するmetadata。

plugin identity、declared capability、configuration need、contributionなどを表す。厳密なschemaは実装しながら決める。

### Plugin Source

Pluginを取得できる場所。

npm package、Git repository、local filesystem path、built-in descriptorなどが例になり得る。Plugin SourceとInstalled Pluginは別物。

### Installed Plugin

user scopeまたはworkspace scopeにinstallされたPlugin。

Installedであることと、すべてのworkspaceやtaskでenabledであることは同義ではない。

### Enabled Plugin

特定scopeでactiveになっているInstalled Plugin。

scopeはuser-wide、workspace-wide、task-specificなどになり得る。正確なscope modelは実装しながら決める。

### Capability

Workyardが検出または利用できる能力。

Capabilityはcore、plugin、runtime、external tool、local configurationなどから来る。

## Integration Terms

### MCP Server Template

MCP serverの設定方法をpublic-safeに表現したもの。

command name、package reference、environment variable nameなどを含められる。credential、account-specific ID、private URLは含めない。

### MCP Profile

use caseごとにMCP Server Templateを組み合わせたもの。

MCP Profileはoptionalであるべき。MCPを使わない利用者が必要としない形にする。

### External Tool

Workyardの外にあり、Workyardが検出、説明、連携できるtool。

MCP server、cross-agent messaging tool、browser tool、runtime-specific utilityなどが例になり得る。

External ToolそのものはPluginではない。

### Managed Tool

Workyardが利用者の代わりにinstall、update、removeするExternal Tool。

Managed Tool supportは、versioning、安全性、uninstall behavior、user consentが明確になった場合だけ追加する。

## Configuration And Safety Terms

### Credential Reference

credentialをどこから取得するかを示す参照。

environment variable nameやkeychain entry nameなどが例。Credential Referenceはpublic templateに置いてよいが、credential valueは置かない。

### Account-Specific Configuration

user account、workspace、team、channel、file、organizationなどに紐づくconfiguration。

このdataは利用者localに置く。publicなWorkyard Repositoryにはcommitしない。

## 混ぜてはいけない言葉

- Workyard RepositoryとUser Workspaceは同じではない。
- Example WorkspaceとUser Workspaceは同じではない。
- PluginとExternal Toolは同じではない。
- Plugin SourceとInstalled Pluginは同じではない。
- Installed PluginとEnabled Pluginは同じではない。
- RuntimeとAgent Roleは同じではない。
- SkillとPluginは同じではない。
- CatalogとPromptは同じではない。
- TaskとRunは同じではない。
- Canonical Repository ReferenceとRepository Cloneは同じではない。
- Repository CloneとTask Worktreeは同じではない。
- Credential Referenceとcredential valueは同じではない。
- Account-Specific Configurationとpublic template dataは同じではない。
