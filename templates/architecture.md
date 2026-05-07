---
project: {project}
module: {name}
type: architecture
status: draft
created: {YYYY-MM-DD}
owner: {who}
supersedes: {previous architecture spec, if any}
tags: []
---

# Architecture: {module}

## Purpose
{What this module exists for. One paragraph.}

## Boundary
- **Owns:** {data, behavior, decisions inside}
- **Does not own:** {explicit — what callers must do themselves}

## Public contract
{Interfaces, endpoints, events. Stable surface. Changes here = breaking change.}

## Internal structure
{Submodules, layering, key abstractions. Diagram if it helps.}

## Invariants
- {must always hold — these are the things that break silently if violated}

## Dependencies
- **Upstream (we depend on):** {list}
- **Downstream (depends on us):** {list}

## Decisions & Trade-offs
- **{decision}** — picked {X} over {Y} because {reason}.

## Non-goals
- {explicit — what this module deliberately does NOT solve}

## Open questions
- {bullet}
