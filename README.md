# SDD — Spec-Driven Development

A pipeline that turns a raw feature idea into a ready-to-execute implementation plan through a structured AI-assisted interview and document chain.

**The core insight:** the smarter the model working on the pipeline, the dumber the model can be on implementation.

Invest intelligence upfront — in the spec. Then any agent can execute: Claude, GPT, a local Ollama model, whatever you have.

---

## The problem with built-in planning modes

Claude Code, Cursor, Copilot Workspace — they all have planning pipelines. But those pipelines are black boxes. You can't see the prompts, can't adapt them to your stack, can't control what questions get asked before implementation starts.

SDD is the same idea, made explicit and portable. Five markdown files. No lock-in.

---

## How it works

You answer questions. AI writes the spec. AI writes the plan. You hand the plan to a coding agent.

```
Interview → Proposal → Requirements → Acceptance Criteria → Constraints → Verification → Plan
```

Each document feeds the next. By the time the coding agent starts, there's nothing left to interpret.

---

## Pipeline steps

| File | Input | Output |
|------|-------|--------|
| `interview.md` | Your answers | `proposal.md` |
| `00-prop-to-req.md` | `proposal.md` | `requirements.md` |
| `01-req-to-ac.md` | `requirements.md` | `acceptance_criteria.md` |
| `02-architect-constraints.md` | `requirements.md` + `acceptance_criteria.md` | `constraints.md` |
| `03-verification.md` | All three above | `verification.md` |
| `04-task-list.md` | All above | `plan.yaml` |

---

## Usage

### 1. Start the interview

Open your agent in the project folder and run:

```
/interview.md
```

Answer questions one by one. The agent writes `proposal.md` when done.

### 2. Run the pipeline

Run each step in order:

```
/00-prop-to-req.md
/01-req-to-ac.md
/02-architect-constraints.md
/03-verification.md
/04-task-list.md
```

Each step reads the outputs of the previous ones.

### 3. Review verification

If `03-verification.md` reports blockers — fix the relevant document manually, then re-run step 3.

### 4. Hand off the plan

Give `plan.yaml` to your coding agent. The plan contains phases, atomic tasks, dependencies, and validation criteria.

---

## Why this works

Most AI coding failures happen before the first line of code — the agent didn't know what to build. SDD front-loads that clarity:

- **Interview** forces you to think through the problem before touching code
- **Requirements** separate what from how
- **Acceptance criteria** define done before implementation starts
- **Constraints** encode architecture decisions the agent must respect
- **Verification** catches contradictions between documents before they become bugs
- **Plan** gives the agent atomic tasks it can execute and validate independently

---

## Model strategy

Use your best model for the pipeline (interview → plan). Use a cheaper or local model for implementation.

The plan produced by the pipeline is specific enough that a weaker model can execute it reliably — because it has no ambiguity to resolve. This makes SDD practical for:

- Cost reduction (GPT-4o for spec, GPT-4o-mini for code)
- Privacy (local Ollama model handles implementation, nothing leaves your machine)
- Offline workflows

---

## Stack compatibility

The pipeline is stack-agnostic. The `02-architect-constraints.md` step is where you inject project-specific context: your tech stack, key entities, existing patterns to follow.

Works with any agent that can read files and follow instructions: Claude Code, Cursor, Aider, Continue, or a raw API call.

---

## Contributing

PRs welcome. If you adapt the prompts for a specific stack or domain, share them.
