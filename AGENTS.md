# Workyard Agent Guide

## Purpose

Workyard is a generic public workspace for AI-assisted work across multiple repositories and worktrees.

Agents working in this repository should keep implementation, examples, and documentation suitable for public open-source use.

## Safety Rules

- Do not add secrets, tokens, credentials, private URLs, customer data, or organization-internal domain knowledge.
- Do not reference private predecessor projects or internal project names.
- Keep `.local/` out of git. It is for local clones, worktrees, run logs, and scratch data.
- Prefer small, reviewable changes that map cleanly to a single issue.
- Document assumptions when defining agent behavior, MCP profiles, or runtime modes.

## Repository Boundaries

Workyard coordinates work in other repositories, but it should not vendor those repositories.

Use `.local/` for disposable local state:

- `.local/repos/<repo-name>/`
- `.local/worktrees/<repo-name>/<task-name>/`
- `.local/runs/<run-id>/`
- `.local/scratch/`

## Public Documentation

Write documentation in English by default because this repository is intended for public use.

## Verification

For documentation-only changes, review rendered Markdown and check links when practical.

For future code changes, add repository-local checks and document them in `docs/` and task acceptance criteria.
