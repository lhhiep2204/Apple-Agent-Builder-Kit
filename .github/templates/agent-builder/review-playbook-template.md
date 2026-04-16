# Review Playbook Template

Use this when generating a review playbook doc for a target project's separated review pipeline.

---

## Template

```markdown
# <Project Name> Review Playbook

## Review Pipeline Overview

```
Code Review Orchestrator
├── Stage 1: Functional Review (business correctness)
│   └── BLOCKER? → REJECT (short-circuit, skip remaining stages)
├── Stage 2: Technical Review (architecture, performance, security)
└── Stage 3: Platform Review [conditional] (Apple-specific: memory, concurrency, SwiftUI)
    └── Only runs when Swift/SwiftUI/UIKit files are changed
```

## What Each Reviewer Checks

### Functional Reviewer
- Acceptance criteria ↔ test ↔ code traceability
- Business rule correctness (verified against code/docs)
- Edge case coverage
- Cross-domain data integrity
- **Short-circuit authority**: BLOCKER finding = immediate REJECT

### Technical Reviewer
- Architecture compliance (module boundaries, layer responsibility)
- API backward compatibility
- Database/migration safety
- Performance implications
- Error handling and logging
- Security (input validation, auth boundaries)

### Platform Reviewer (Apple-specific)
- Memory management (retain cycles, actor isolation)
- UI thread safety (MainActor, background dispatch)
- SwiftUI recomposition analysis
- Concurrency correctness (Sendable, task cancellation)
- Accessibility and localization completeness
- Platform capability usage (entitlements, privacy descriptions)

## Severity Levels

| Severity | Meaning | Action |
|----------|---------|--------|
| **BLOCKER** | Broken business logic, data corruption risk, security vulnerability | Must fix. Short-circuits pipeline if found in functional review. |
| **MAJOR** | Architecture violation, significant performance issue, missing test coverage | Must fix before merge. |
| **MINOR** | Style inconsistency, non-critical improvement opportunity | Fix preferred, can defer with justification. |
| **SUGGESTION** | Enhancement idea, alternative approach consideration | Optional. |

## Verdicts

| Verdict | Meaning |
|---------|---------|
| **PASS** | All stages passed. Safe to merge. |
| **REVISE** | Issues found but fixable. Implement revision checklist and re-submit. |
| **REJECT** | Fundamental problem (business logic, architecture). Requires re-implementation or significant rework. |

## Evidence Standard in Reviews

Every finding must include:
- **Code anchor**: exact file path and line reference
- **Evidence type**: code-backed, spec-backed, `[ASSUMPTION]`, `[NEEDS CLARIFICATION]`
- **Severity**: blocker / major / minor / suggestion
- **Fix suggestion**: concrete recommendation with code example when applicable

## Blast-Radius Review

For high-impact changes, request expanded review scope:
- Changes to shared/core modules → require full technical review of dependent modules
- Database schema changes → require migration safety analysis
- API contract changes → require backward compatibility check
- Cross-module refactors → require dependency-ordered review

## Escalation

If reviewers disagree (conflicting findings between Functional and Technical):
1. The Code Review Orchestrator mediates
2. If unresolvable, escalate to the Dev Orchestrator
3. Present the conflict to the user via `vscode_askQuestions` with both positions and a recommended resolution
```

---

## Generation Rules

- Replace `<Project Name>` with actual project name
- Adjust pipeline diagram to match what was actually generated (e.g., if Platform Reviewer is conditionally triggered)
- This doc helps teams understand and trust the review pipeline behavior
