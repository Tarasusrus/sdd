Role

You are an AI coding agent creating an implementation plan from a specification.

Input

- @file:proposal.md
- @file:requirements.md
- @file:acceptance_criteria.md
- @file:constraints.md

Task

Analyze the specification and produce an EXECUTION PLAN with:
- Phases — logical groupings of work that can be validated independently
- Tasks — atomic units of work within each phase
- Dependencies — what must complete before each task can start
- Validation criteria — how to verify each task is complete
- Checkpoints — points where human review is recommended

Plan Requirements

Task Granularity:
- Each task completable in a single focused effort
- Tasks produce a verifiable artifact (file, test passing, etc.)
- Tasks small enough to rollback if wrong

Dependency Rules:
- No circular dependencies
- Minimize cross-phase dependencies
- Migrations before business logic
- Interfaces before implementations

Checkpoint Placement — after:
- DB schema / migration changes
- Core domain model completion
- Each major component integration
- Test suite completion
- Final integration

Output Format

plan:
  name: "{feature name}"
  phases:
  - id: phase-1
    name: "{phase name}"
    description: "{what this phase accomplishes}"
    tasks:
    - id: task-1.1
      name: "{task name}"
      description: "{what to do}"
      artifact: "{file path or outcome}"
      depends_on: []
      validation: "{how to verify completion}"
    checkpoint:
      description: "{what to review}"
      criteria: ["{criterion 1}", "{criterion 2}"]

Constraints

- Maximum 5 phases
- Maximum 7 tasks per phase
- Every task must have a validation criterion
- Every phase must end with a checkpoint
- Report if result does not fit constraints

IMPORTANT: do not start implementing the tasks, only output the task list

Output File

Write the result to spec/plan.yaml
