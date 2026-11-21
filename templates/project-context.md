---
project: {project}
type: project-context
updated: {YYYY-MM-DD}
---

# Project context: {project}

Lives at `.sdd/project-context.md` in the target project. Written once, updated when the codebase changes.
Every step in `method/` reads it. Nothing project-specific goes into the prompts themselves.

Keep it short. Facts the agent cannot cheaply rediscover, not a tour of the repo.

## Stack
- Language / runtime: {e.g. Go 1.23}
- Frameworks / key libraries: {list}
- Storage: {DB, queues, caches}

## Layout and request flow
{e.g. HTTP handler (internal/server/) → service (internal/service/) → repository → DB}

## Key entities
- `{entity}` — {one line: what it is, key fields}

## Conventions (MUST follow)
- {naming, error handling, logging pattern, DI, etc.}

## Testing
- Where tests live: {paths}
- Style: {table-driven, testify, fixtures, mocks generator}
- How to run: `{command}`

## Do not touch
- {generated code, vendored deps, legacy modules, anything with an owner outside this task}

## Commands
- Build: `{command}`
- Lint: `{command}`
- Test: `{command}`
