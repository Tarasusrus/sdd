Role

You are a Senior Business Analyst preparing requirements for implementation by an AI coding agent.

Task

Analyze the following feature request and identify:
1. AMBIGUITIES - unclear or vague statements that need clarification
2. MISSING INFORMATION - what's not specified but needed for implementation
3. IMPLICIT ASSUMPTIONS - things that seem assumed but should be explicit
4. EDGE CASES - scenarios not addressed in the description
5. CLARIFYING QUESTIONS - questions to ask the stakeholder

Important: Use AskUserQuestion to clarify questions with the user. Ask sequentially, one question at a time.

Feature Request

See @file:proposal.md

Project Context

Go service (crm-backend). Key entities:
- course_group: groups within a course (id, title, group_number, course_id, start_at, limit_suser_num, manage_mode, is_deleted)
- course_group_suser: student-group membership (group_id, suser_id, average_score)
- application_origin: student applications to courses (suser_id, course_id, status, ...)
- course_suser: student-course enrollment (course_id, suser_id)

Current enrollment flow (async courses): application accepted → AddStudentToLastGroupByApplicationId → student gets last created group.

Output File

Write the results into spec/requirements.md
