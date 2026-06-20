# Workyard

Workyard is a local workspace for running AI-assisted work across many repositories, worktrees, prompts, MCP servers, and agent runtimes.

It is designed to keep the execution workspace separate from the repositories being changed:

- cloned repositories and worktrees live under `.local/`
- reusable agent guidance lives under `catalog/`, `prompts/`, and `agents/`
- runtime-specific launch guidance lives under `runtimes/`
- MCP configuration examples live under `mcp/`
- planned work lives under `tasks/`

## Status

This repository is in early design. The initial goal is to define the operating model and grow the implementation incrementally through small issues.

## Design Principles

- Keep the project generic and suitable for public open-source use.
- Do not include private organization details, customer details, secrets, or domain-specific internal knowledge.
- Treat target repositories as separate projects. Workyard should coordinate work without owning their source code.
- Prefer explicit catalogs and checks over relying on an agent to infer engineering standards from scattered context.
- Make local state disposable. Generated clones, worktrees, run logs, and scratch files belong under `.local/`.

## Repository Layout

```text
.
├── .local/          # local-only clones, worktrees, runs, and scratch files
├── agents/          # reusable agent roles and behavior profiles
├── catalog/         # agent-readable engineering catalog
├── docs/            # operating model and project documentation
├── mcp/             # MCP server and profile examples
├── prompts/         # reusable prompt templates
├── runtimes/        # runtime profiles for agent tools
├── scripts/         # bootstrap and utility scripts
└── tasks/           # task templates and active task notes
```

## License

MIT License. See [LICENSE](LICENSE).
