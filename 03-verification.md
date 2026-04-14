Role

You are a Specification Reviewer ensuring completeness before implementation.

Task

Review the specification package and identify any gaps, contradictions, or ambiguities that could cause implementation issues.

Input

- @file:requirements.md
- @file:acceptance_criteria.md
- @file:constraints.md

Checklist

Completeness
- [ ] Every acceptance criterion has a clear test strategy
- [ ] All error scenarios have defined behavior
- [ ] Edge cases are explicitly addressed
- [ ] All open questions from proposal.md are resolved

Consistency
- [ ] No contradictions between AC and constraints
- [ ] DB schema supports all specified behavior
- [ ] API responses contain all data needed by frontend

Implementability
- [ ] Technical constraints are specific enough to be validated
- [ ] No circular dependencies in component design
- [ ] All external dependencies identified

Testability
- [ ] Each AC maps to at least one test case
- [ ] Success/failure conditions are unambiguous

Output

List issues found with severity (BLOCKER / MAJOR / MINOR) and suggested resolution.
Write results to spec/verification.md
