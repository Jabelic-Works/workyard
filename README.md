# Workyard

Workyard is a local workspace for running AI-assisted work across many repositories, worktrees, prompts, MCP servers, plugins, and agent runtimes.

このrepositoryはWorkyard自体を開発するsource repositoryです。利用者がinstall後に使うUser Workspaceそのものではありません。

User Workspaceの形は `examples/workspace/` に置き、root directoryはWorkyardの開発に必要なsource、docs、examplesを中心に保ちます。

## Status

This repository is in early design.

当面の目標は、Workyard RepositoryとUser Workspaceを混同せず、reviewしやすいPRで漸進的に実装できる状態にすることです。

## Design Principles

- public OSSとして読まれて困らないgenericなprojectにする。
- private organization details、customer details、secret、domain-specific internal knowledgeを入れない。
- target repositoryは別projectとして扱う。Workyardは作業をcoordinationするが、target repositoryのsource codeを所有しない。
- agentに散らばったcontextから推測させず、catalogとcheckを明示する。
- local stateはdisposableにする。Generated clone、worktree、run log、scratch fileは `.local/` に置く。
- featureは原則pluggableにする。1つのruntimeだけを使う利用者も対象から外さない。

## Documentation

- [ユビキタス言語](docs/ubiquitous-language.md)
- [Operating Model](docs/operating-model.md)
- [User Workspace Layout案](docs/workspace-layout.md)

## Repository Layout

```text
.
├── AGENTS.md          # このrepositoryで作業するAI agent向けのguide
├── docs/              # design noteとproject documentation
├── examples/          # User Workspaceなどのtracked example
├── packages/          # Workyard package source
└── scripts/           # development / bootstrap utility
```

`.local/` はlocal development中に存在してよいが、gitではignoreされ、commitしない。

## License

MIT License. See [LICENSE](LICENSE).
