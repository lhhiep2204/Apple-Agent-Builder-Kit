# Constitution Template

Use this when generating a constitution instruction file for a target project. Replace all placeholders with project-specific content derived from the analyzer.

---

## Template

```markdown
---
description: "Immutable governance rules for <project-name> agent ecosystem. Overrides any agent-specific instruction when in conflict."
applyTo: "**"
---

# <Project Name> Constitution

## Preamble

This constitution defines the non-negotiable rules governing the <project-name> agent ecosystem. All agents must comply. When an agent-specific instruction conflicts with this document, the constitution wins.

## Article I: Scope & Boundaries

**In scope**: <What this agent ecosystem covers — e.g., feature development, testing, code review, investigation for the <project-name> <platform> application>
**Out of scope**: <What it does not cover — e.g., infrastructure provisioning, CI/CD pipeline changes, App Store submission>

## Article II: Evidence Standard

Every business-rule claim, investigation finding, and review verdict must be backed by evidence:

| Label | Meaning |
|-------|---------|
| (no label) | Verified against code or documentation |
| `[USER-CONFIRMED]` | Explicitly confirmed by a human via vscode_askQuestions |
| `[ASSUMPTION]` | Reasonable inference, not yet verified — must be resolved before implementation |
| `[NEEDS CLARIFICATION]` | Ambiguous or unknown — requires investigation or user input |

**Rules**:
- Investigators mark uncertain findings as `[ASSUMPTION]`
- Implementors must verify all `[ASSUMPTION]` labels before coding
- Reviewers must challenge any `[ASSUMPTION]` that reaches review without resolution
- The orchestrator tracks assumption resolution across phases

## Article III: Review Pipeline

The project uses a separated multi-stage review pipeline:

- Stage 1 (Functional Review) runs first. BLOCKER findings short-circuit the pipeline.
- Technical and Platform reviews only run after functional approval.
- Every finding includes: code anchor, evidence type, severity, fix suggestion.
- Review verdicts: PASS / REVISE / REJECT. REJECT requires re-implementation.

## Article IV: Change Governance

**Blast-radius assessment** is mandatory before implementation:
- Changes to shared/core modules require explicit impact analysis
- Database/schema changes require migration safety review
- API changes require backward compatibility check
- Cross-module changes require dependency-ordered execution plan

**Confirmation checkpoint**: The orchestrator must confirm the investigation report and implementation plan with the user before delegating to the implementor for high-impact changes (>5 files, core module, shared API).

## Article V: Specification Discipline

Non-trivial features (>3 files, cross-module, business logic changes) must go through the spec-driven pipeline:
1. Refine user input → structured goal/constraints
2. Specify feature → PRD with testable ACs
3. Plan implementation → architecture approach with risk assessment
4. Generate tasks → ordered execution breakdown

Specs live in `specs/<feature-id>/` and are updated as implementation reveals new constraints.

## Article VI: Context Persistence

- Investigation findings persisted to session memory before implementation begins
- Multi-step plans tracked via `manage_todo_list`
- Key decisions recorded with rationale in session memory
- Progress checkpoints after each major phase for long tasks
- Context compaction: summarize completed phases, discard exploration narrative

## Article VII: Escalation

Escalate to human decision-making when:
- Two `[ASSUMPTION]` labels conflict with each other
- Implementation cost exceeds reasonable scope (detected by task count >15)
- Review pipeline produces REJECT twice on the same change
- Architectural decision has irreversible consequences not covered by existing conventions
- User request contradicts existing project conventions

Use `vscode_askQuestions` for escalation — present the conflict, options, and a recommended path.
```

---

## Phase -1 Gates

Include this section in the constitution. These gates MUST pass before any implementation begins:

```markdown
## Phase -1 Gates

Before writing any implementation code, verify all four gates:

### Simplicity Gate
- Is this the simplest approach that satisfies the requirement?
- Are we introducing unnecessary abstraction or indirection?
- Could an existing pattern or utility solve this with less code?

### Duplication Gate
- Does this duplicate existing logic elsewhere in the codebase?
- Can existing code be extended or reused instead of writing new code?
- Search the codebase for similar patterns before creating new ones.

### Business Logic Gate
- Are business rules verified against source code, documentation, or domain expert confirmation?
- Are all domain assumptions labeled with evidence type?
- Does the proposed change preserve existing business invariants?

### Impact Gate
- Is the blast radius understood? Which modules, tests, and downstream consumers are affected?
- Is the change reversible? If not, is there a rollback plan?
- Does the dependency order prevent partial-completion failures?
```

---

## Generation Rules

- Replace all `<placeholders>` with project-specific content from the analyzer
- Constitution is an instruction file (`applyTo: "**"`), NOT an agent
- Keep concise: target 80-120 lines. Every line must earn its place
- Do not duplicate content that belongs in the project context instruction
- The constitution focuses on governance rules; the project context instruction covers project facts
