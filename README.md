# SDD — Spec-Driven Development

Personal methodology for spec-driven AI development plus a public archive of sanitized examples.

The core idea: a spec is intent that survives the conversation. The chat closes, the channel scrolls, the author forgets. The spec stays. Code answers *how*, git answers *when*, the spec answers *why* — and *why* decays the fastest.

This repo holds:

- **`method/`** — the prompt pipeline that turns a raw idea into an executable plan
- **`templates/`** — spec shapes for the four cases that actually show up
- **`examples/`** — sanitized, real artifacts (private archive lives in `sdd-archive`)
- **`INDEX.md`** — index of archived specs

---

## When to use it

Not on every task. SDD is overhead, and overhead on a typo fix is how teams come to hate the method.

Rule of thumb: **if you'd be annoyed at the AI for misunderstanding, write a spec. Otherwise don't.**

You don't have to decide by hand: start every task with `method/sdd.md` and it picks the route.

---

## Entry point: `method/sdd.md`

```
/sdd <task in your own words>
```

1. **Project context.** First run in a project creates `.sdd/project-context.md` from the code (stack, layout, conventions, test commands, do-not-touch). Every later step reads it, so prompts stay project-agnostic.
2. **Triage.** Reads the code the task points at, scores it against a few signals (3+ files, schema / public API, >30 min, a "why" decision, vague requirements, state migration) and picks a route:

| Route | When | Path |
|-------|------|------|
| `direct` | no signals | implement → tests → done |
| `change` | 1–2 signals, modifies existing behaviour | clarify (≤5 questions) → `change-delta` → plan → implement → converge → archive |
| `feature` | 3+ signals or new capability | full pipeline below |
| `migration` / `architecture` | state move / module boundary | full pipeline with the matching template |

3. **Gates.** You confirm the route, and you say "go" after the plan. Checkpoints in the plan stop for review.
4. **Upgrades, not downgrades.** If a task turns out bigger mid-flight, the route is upgraded and the missing steps run. It is never cut back.
5. **Resume.** `spec/route.md` and statuses in `spec/plan.yaml` let a new session continue where the last one stopped.

---

## The pipeline (`method/`)

```
sdd → interview → requirements → acceptance → constraints → verification → plan → implement → converge → archive
```

| Step | Input | Output |
|------|-------|--------|
| `method/sdd.md` | the task | `spec/route.md`, `.sdd/project-context.md` (once) |
| `method/interview.md` | your answers | `proposal.md` |
| `method/00-prop-to-req.md` | `proposal.md` | `requirements.md` |
| `method/01-req-to-ac.md` | `requirements.md` | `acceptance_criteria.md` (with `AC-x.y` IDs) |
| `method/02-architect-constraints.md` | `requirements.md` + `acceptance_criteria.md` | `constraints.md` |
| `method/03-verification.md` | all of the above | `verification.md` |
| `method/04-task-list.md` | all of the above (or `change.md`) | `plan.yaml` — tasks with `covers` and `status` |
| `method/05-implement.md` | `plan.yaml` | code, one commit per task, statuses updated |
| `method/06-converge.md` | spec + diff | `converge.md` — every AC mapped to evidence; gaps become tasks |
| `method/07-archive.md` | shipped feature | `archive/<project>/<date>-<slug>/` + `retro.md` + INDEX row |

Run each step as a slash command in your agent. Each step reads what the previous step produced. All artifacts for the active task live in `spec/`.

If `03-verification.md` reports blockers — fix the upstream doc, re-run.
If `06-converge.md` reports gaps — they are appended to the plan as tasks, run `05` again.

---

## Templates (`templates/`)

Four spec shapes cover the common cases, plus the project context:

- **`project-context.md`** — per-project facts every step reads; lives at `.sdd/project-context.md` in the target project
- **`feature-spec.md`** — new capability, the long-lived doc
- **`change-delta.md`** — modification of an existing feature, references parent spec
- **`migration.md`** — moving between two states (storage, contract, infra)
- **`architecture.md`** — module boundary and contract

Every template has frontmatter for indexing and a mandatory **Decisions & Trade-offs** section. The decisions section is the part that earns its keep a year later.

---

## Archive

This repo's `examples/` and `INDEX.md` only hold sanitized, share-safe specs.

Real working specs — with business context, internal names, NDA material — live in the **private `sdd-archive` repo**, mirroring the same structure (`<project>/<YYYY-MM-DD>-<slug>/`).

Each archived feature must include `retro.md`: what worked, what didn't, what to add to the templates. This is the feedback loop that keeps the method itself from rotting.

---

## Model strategy

Use the strongest available model for `method/` (intent, plan, review in 05 and 06). Use a cheaper or local model for implementation — the plan removes ambiguity, so a weaker executor is fine. `05-implement.md` makes this concrete: each task goes to a fresh executor with only its slice of the spec, and the strong model reviews the result.

This unlocks:
- Cost — strong model for spec, cheap for code
- Privacy — local model for implementation, nothing leaves the machine
- Offline — same

---

## Stack compatibility

Pipeline is stack-agnostic. Project-specific context — tech stack, key entities, conventions to respect — goes into `.sdd/project-context.md`, never into the prompts.

Works with any agent that reads files and follows instructions: Claude Code, Cursor, Aider, Continue, raw API.

---

## Related work

This repo is a personal pipeline, not a framework. If you want a packaged tool: GitHub **Spec Kit**, Fission AI **OpenSpec**, **BMAD-METHOD**, **GSD**, AWS **Kiro**. They solve overlapping problems with more tooling. The methodology here predates and post-dates whatever tool is current.

---

## Contributing

Improvements to `method/` and `templates/` welcome. Sanitized examples for `examples/` welcome. The private archive is private.
