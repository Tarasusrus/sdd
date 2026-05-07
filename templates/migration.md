---
project: {project}
type: migration
status: draft
created: {YYYY-MM-DD}
owner: {who}
tags: []
---

# Migration: {from-state → to-state}

## Why
{What forced the move. Cost / risk / capability gap.}

## From → To
- **From:** {current state — be specific about versions, schemas, contracts}
- **To:** {target state}

## Strategy
{Big-bang? Phased? Dual-write? Shadow read?}

## Phases
1. {phase — entry criteria, exit criteria}
2.
3.

## Rollback plan
{What we do if step N fails. Required, not optional.}

## Data integrity checks
- [ ] {invariant that must hold before/after}

## Risks
- {risk} → {mitigation}

## Decisions & Trade-offs
- **{decision}** — {why}.

## Done when
- [ ] {observable signal — not "code is merged"}
