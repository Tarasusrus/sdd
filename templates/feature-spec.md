---
project: {project}
feature: {kebab-case-slug}
type: feature
status: draft   # draft | active | shipped | archived
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
owner: {who}
tags: []
---

# {Feature title}

## Why
{One paragraph. The problem. Cost of not solving it. This is the part that disappears first if not written down.}

## What
{User-visible behavior. No tech stack, no implementation. "When X, the user sees Y".}

## Scope — in
- {bullet}

## Scope — NOT in
- {bullet — required, not optional. Half of all rework starts with "I thought we'd also do X".}

## Acceptance checklist
- [ ] {testable, binary yes/no}
- [ ] {testable, binary yes/no}

## Constraints
{Hard limits: latency, compatibility, security, regulatory.}

## Decisions & Trade-offs
- **{decision}** — picked {X} over {Y, Z} because {reason}.

## Open questions
- {bullet — explicit. Empty list means "none", not "haven't thought".}

## Links
- {related specs, ADRs, tickets, PRs}
