# Workyard Agent Guide

## 目的

このファイルは、Workyard Repositoryを開発するAI agent向けの作業ガイドです。

Workyardは、複数repository / worktree / AI runtimeをまたぐ作業場を作るためのOSS projectです。このrepository自体はUser Workspaceではありません。Workyard Repository、User Workspace、Example Workspace、Plugin、External Toolを混同しないでください。

用語は [docs/ubiquitous-language.md](docs/ubiquitous-language.md) を優先します。

## 基本方針

- 返答とissue/PR向けの説明は原則日本語で書く。
- public OSSとして読まれて困らない内容だけを書く。
- private predecessor project、社内固有名、顧客情報、credential、private URL、account-specific configurationを入れない。
- 初期段階ではIssueを増やしすぎない。必要になったら、具体的なPRや利用者アウトカムからIssueを作る。
- 技術的な詳細は、小さなPRを書きながら決める。

## Repository Boundary

Workyard Repositoryは、Workyard自体を開発するsource repositoryです。

root directoryを実利用者のUser Workspaceの形に寄せすぎないでください。User Workspaceの例が必要な場合は、`examples/`配下に置き、Example Workspaceであることを明示します。

`.local/` はlocal-onlyです。clone、worktree、run log、scratch file、generated configなどを置けますが、commitしてはいけません。

## Plugin-first Direction

Workyardはplugin-firstです。

- coreは薄く保つ。
- featureは原則pluginとして提供する。
- External ToolそのものをPluginと呼ばない。
- single-runtime利用者を対象から外さない。
- user-specific dataはpublic repositoryに置かない。

安定した方針はrepository docsまたはGitHub Discussionに記録します。Wikiは情報置き場として使いません。

- [プラグイン優先アーキテクチャの意思決定](https://github.com/Jabelic-Works/workyard/discussions/54)
- [プラグイン優先アーキテクチャの実装候補](https://github.com/Jabelic-Works/workyard/discussions/53)

## Issue / PR Discipline

- 1 PRは小さく、reviewしやすくする。
- Issue titleは、技術作業ではなく利用者や開発者のアウトカムを表す。
- 技術選定や内部APIの詳細は、必要になるまでIssue化しない。
- PR descriptionには、何が変わるか、なぜ必要か、どう確認したかを書く。
- 関連Issueがある場合はPRに `Closes #...` を入れる。

## Documentation

- このrepositoryの開発方針や用語は日本語で書いてよい。
- Runtime、Plugin、External Tool、MCP、Skillなどの語は、辞書に合わせて使う。
- Workyard Repository向けの説明とUser Workspace向けの説明を分ける。

## Verification

documentation-only changeでは最低限 `git diff --check` を実行します。

code changeでは、repository-localなcheckが追加された時点で、そのcheckを使います。checkが未整備の間は、PR descriptionに確認した内容を明記します。
