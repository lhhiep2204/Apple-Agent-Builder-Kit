# Task Breakdown Template

Use this when generating a tasks-template for a target project's spec-driven pipeline. The generated template is used by the Generate-Tasks skill to produce ordered task breakdowns from approved plans.

---

## Template

```markdown
# Tasks: <Feature Title>

**Plan**: specs/<feature-id>/plan.md
**Status**: not-started | in-progress | completed

## Task List

| # | Task | Files | Validation | Complexity | Depends On | Status |
|---|------|-------|-----------|------------|------------|--------|
| 1 | <task description> | <file paths> | <how to verify> | S/M/L | — | not-started |
| 2 | <task description> | <file paths> | <how to verify> | S/M/L | Task 1 | not-started |
| 3 | <task description> | <file paths> | <how to verify> | S/M/L | Task 1 | not-started |
| 4 | <task description> | <file paths> | <how to verify> | M/L | Tasks 2,3 | not-started |

## Execution Notes

- Tasks with no dependencies can be executed in any order
- Each task should be independently verifiable (build + tests pass after completion)
- Update status in `manage_todo_list` as tasks progress
- If a task reveals new complexity, update the plan and regenerate remaining tasks

## Validation Checkpoints

- After Task <N>: <intermediate validation — build, test subset, manual check>
- After all tasks: Full test suite + lint + build verification
```

---

## Generation Rules

- Each task must be small enough for a single focused implementation session
- Validation criteria must be concrete and executable (build command, test command, manual check)
- Complexity estimate (S/M/L) helps the orchestrator plan execution
- Dependency column prevents out-of-order execution
- Tasks feed directly into `manage_todo_list` for progress tracking
- The template lives in `.github/templates/<prefix>/` in the generated bundle

## Cross-Reference Wiring (Required)

The generated tasks template must be referenced by the Generate-Tasks skill and the Dev Orchestrator. Full wiring rules and verification checklist in SKILL.md > Template Cross-Reference Requirements. Orphan check in `agent-audit-rubric.md` > Template Orphan Check.
