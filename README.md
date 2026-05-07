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

Concrete threshold:
- More than one module touched
- More than 30 minutes of implementation
- Decision a future reader will ask "why" about

Below that — prompt the agent and move on.

---

## The pipeline (`method/`)

```
interview → proposal → requirements → acceptance → constraints → verification → plan → archive
```

| Step | Input | Output |
|------|-------|--------|
| `method/interview.md` | your answers | `proposal.md` |
| `method/00-prop-to-req.md` | `proposal.md` | `requirements.md` |
| `method/01-req-to-ac.md` | `requirements.md` | `acceptance_criteria.md` |
| `method/02-architect-constraints.md` | `requirements.md` + `acceptance_criteria.md` | `constraints.md` |
| `method/03-verification.md` | all of the above | `verification.md` |
| `method/04-task-list.md` | all of the above | `plan.yaml` |
| `method/05-archive.md` | shipped feature | `archive/<project>/<date>-<slug>/` + `retro.md` + INDEX row |

Run each step as a slash command in your agent. Each step reads what the previous step produced.

If `03-verification.md` reports blockers — fix the upstream doc, re-run.

---

## Templates (`templates/`)

Four shapes cover the common cases:

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

Use the strongest available model for `method/` (intent, plan). Use a cheaper or local model for implementation — the plan removes ambiguity, so a weaker executor is fine.

This unlocks:
- Cost — strong model for spec, cheap for code
- Privacy — local model for implementation, nothing leaves the machine
- Offline — same

---

## Stack compatibility

Pipeline is stack-agnostic. Inject project-specific context at `method/02-architect-constraints.md` — tech stack, key entities, conventions to respect.

Works with any agent that reads files and follows instructions: Claude Code, Cursor, Aider, Continue, raw API.

---

## Related work

This repo is a personal pipeline, not a framework. If you want a packaged tool: GitHub **Spec Kit**, Fission AI **OpenSpec**, **BMAD-METHOD**, **GSD**, AWS **Kiro**. They solve overlapping problems with more tooling. The methodology here predates and post-dates whatever tool is current.

---

## Contributing

Improvements to `method/` and `templates/` welcome. Sanitized examples for `examples/` welcome. The private archive is private.
