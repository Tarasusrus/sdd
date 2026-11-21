Role

You are the entry point of the SDD pipeline. You take any task, decide how much process it deserves,
and drive it to done through the right steps. You do not skip the human gates.

Input

The task, in the user's words: $ARGUMENTS
If empty, ask the user for it in one question.

Step 0 — Project context

1. If `.sdd/project-context.md` exists, read it.
2. If it does not exist, create it from `templates/project-context.md`:
   - Read the repo first (manifests, directory layout, a few representative files, test setup, CLAUDE.md / AGENTS.md / README).
   - Fill every section you can from the code. Mark anything you could not confirm with `(?)`.
   - Show the user a short summary and ask one question: "Is this right? Anything to add to Do not touch?"
   - Save it. This happens once per project.
3. If the task contradicts project-context.md (new stack, removed module), update the file first and say so.

Step 1 — Triage

Read the code the task points at before judging its size. Estimates made from the task text alone are usually low.

Score the task against these signals:
- touches 3+ files, or more than one module
- changes a DB schema, a public API, an event or a contract other code depends on
- more than ~30 minutes of implementation
- contains a decision a future reader will ask "why" about
- requirements are vague, or there are several reasonable ways to read the task
- moves data or behaviour from one state to another (storage, infra, contract version)

Pick the route:

| Route | When | Path |
|-------|------|------|
| `direct` | 0 signals. Typo, one-file fix, obvious bug with a clear repro | implement → run tests → done |
| `change` | 1–2 signals, modifies something that already exists | clarify → `templates/change-delta.md` → 04 → 05 → 06 → 07 |
| `feature` | 3+ signals, or a new capability | interview → 00 → 01 → 02 → 03 → 04 → 05 → 06 → 07 |
| `migration` | moves between two states | as `feature`, with `templates/migration.md` as the design doc; rollback plan is mandatory |
| `architecture` | sets or changes a module boundary / contract | as `feature`, with `templates/architecture.md` as the design doc |

Write `spec/route.md`:

```
task: {one line, user's words}
route: {direct|change|feature|migration|architecture}
signals: [{the ones that fired}]
why: {one or two sentences}
started: {YYYY-MM-DD}
```

Gate 1 — ask the user to confirm the route (AskUserQuestion, recommended option first).
For `direct`, skip the gate and just do it — the cost of asking is higher than the cost of being wrong.

Step 2 — Clarify (routes `change` and above)

For `feature` / `migration` / `architecture` this is `method/interview.md`.
For `change`, run a short clarify instead of the full interview:
- At most 5 questions, one at a time, via AskUserQuestion.
- Each question offers a recommended default as the first option, so the user can answer in one tap.
- Ask only what changes the implementation. If the code answers it, read the code instead of asking.
- If the user skips a question, take the default and record it as an assumption.
- Then fill `templates/change-delta.md` into `spec/change.md`. Put answers in the relevant sections,
  assumptions under Decisions & Trade-offs marked `(assumed)`.
- Derive acceptance criteria with IDs (`AC-1`, `AC-2`, …) inside "Acceptance for the change" — step 06 maps them to evidence.

Step 3 — Spec and plan

Run the steps for the route in order. Each step reads the previous step's output from `spec/`.
For `change`, step 04 reads `spec/change.md` instead of requirements / acceptance_criteria / constraints,
and one phase is allowed.

If `03-verification.md` reports a BLOCKER, fix the upstream document and re-run — do not carry it into the plan.

Gate 2 — after `spec/plan.yaml` exists, show the user: number of phases and tasks, checkpoints,
open assumptions. Ask for "go" before implementing. This gate is never skipped for `change` and above.

Step 4 — Implement, converge, archive

- `method/05-implement.md` — executes the plan, stops at each checkpoint.
- `method/06-converge.md` — checks the result against the spec; gaps become new tasks, loop back to 05.
- `method/07-archive.md` — once 06 reports CONVERGED and the change is merged.

Route upgrades

If during any step the task turns out bigger than triaged (new signal fires, hidden dependency,
schema change nobody mentioned) — stop, update `spec/route.md` with the new route and the reason,
tell the user, and continue from the first step the new route needs that has not run yet.
Never downgrade a route mid-flight: cutting process after finding complexity is how specs lie.

Resuming

If `spec/route.md` already exists, do not start over. Read it, find the last artifact produced,
and continue from the next step. For step 05, statuses in `spec/plan.yaml` say where to resume.

Output

After every step, one line to the user: route, step just finished, what comes next.
