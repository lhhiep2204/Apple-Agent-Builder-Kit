# Feature Specification Template

Use this when generating a spec-template for a target project's spec-driven pipeline. The generated template is used by the Specify-Feature skill to produce feature PRDs.

---

## Template

```markdown
# Feature: <Feature Title>

**ID**: <feature-id>
**Status**: draft | in-review | approved | in-progress | completed
**Author**: <who created the spec>
**Date**: <creation date>

## Problem Statement

<What problem does this feature solve? Who is affected? What is the current behavior vs desired behavior?>

## Scope

**In scope**:
- <What this feature includes>

**Out of scope**:
- <What this feature explicitly does NOT include>

## Acceptance Criteria

Each criterion must be independently testable:

1. **AC-1**: <Given ... When ... Then ...>
2. **AC-2**: <Given ... When ... Then ...>
3. **AC-3**: <Given ... When ... Then ...>

## Domain Rules

- <Business rule 1> [evidence type]
- <Business rule 2> [evidence type]

## Edge Cases

| # | Scenario | Expected Behavior | Priority |
|---|----------|-------------------|----------|
| 1 | <edge case> | <expected> | must-handle / nice-to-have |

## Dependencies

- <Dependency on other features, modules, or external services>

## Technical Constraints

- <Platform requirements, performance targets, backward compatibility needs>

## Open Questions

- <Unresolved items labeled [NEEDS CLARIFICATION]>
```

---

## Generation Rules

- The generator produces this template customized for the target project's domain
- ACs must be testable — not vague descriptions
- Domain rules must include evidence labels per the evidence standard
- Edge cases should be populated from investigator findings when available
- The template lives in `.github/templates/<prefix>/` in the generated bundle
