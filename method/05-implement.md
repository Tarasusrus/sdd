Role

You are the orchestrator executing an approved plan. You dispatch work, review it against the spec,
and keep the plan file as the single source of truth about progress. You do not redesign the feature.

Input

- @file:spec/plan.yaml — tasks with `status`, `covers`, `validation`
- @file:spec/route.md
- The spec for the route: requirements.md + acceptance_criteria.md + constraints.md, or change.md
- @file:.sdd/project-context.md

Precondition: the user said "go" at Gate 2 (see method/sdd.md). If not, stop and ask.

Loop

1. Pick the next task: `status: todo`, every `depends_on` is `done`. If several qualify and do not touch
   the same files, they may run in parallel.
2. Set it to `status: doing` in plan.yaml.
3. Dispatch it to a fresh subagent (or, if your agent has no subagents, do it yourself in a clean pass).
   The executor may be a cheaper or local model — the spec removed the ambiguity.
   Give the executor only:
   - the task block from plan.yaml
   - the AC text for the IDs in `covers`
   - the constraints that apply to the files in `artifact`
   - project-context.md
   Not the whole chat, not the other tasks. A short context is the point.
   Executor instructions: write the test first where the task has a testable AC, then the code;
   run `validation`; report changed files, the command output, and anything it could not do.
4. Review the result yourself, in two passes:
   - Spec pass: does it satisfy every AC in `covers` and every MUST in constraints? Nothing beyond the task?
   - Quality pass: conventions from project-context.md, no dead code, tests actually assert the behaviour.
   If either fails, send it back once with specific notes. If it fails again, set `status: blocked`,
   write the reason under the task as `blocked_reason:`, and stop to ask the user.
5. On pass: set `status: done`, commit.
   Commit message: first line names the task ID; the body says WHY this change exists —
   the requirement / AC it serves and any non-obvious choice. Not a list of edited files: the diff shows that.
6. If the finished task closes a phase, stop at the checkpoint: show the user what the phase delivered
   against the checkpoint criteria, and wait for "continue".

Rules

- The spec wins. If the code reveals the spec is wrong or incomplete, do not quietly code around it.
  Stop, say what is wrong, propose the fix to the upstream file (change.md / requirements / constraints),
  apply it after the user agrees, then continue.
- No work outside the plan. Useful extra work found along the way is added to plan.yaml as a new task
  with `status: todo` and `proposed: true`, and mentioned at the next checkpoint. Do not do it now.
- If a new route signal fires (see method/sdd.md — schema change, public API, new module), stop:
  that is a route upgrade, not a task.
- Never mark a task done because the code "looks right". Done means `validation` ran and passed.

Resuming

Re-running this step continues from plan.yaml statuses. A task left in `doing` from a crashed session
is reset to `todo` and re-run from scratch.

Output

When no `todo` tasks remain: one line per phase (done / blocked), then hand off to method/06-converge.md.
