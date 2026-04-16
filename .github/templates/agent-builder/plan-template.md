# Implementation Plan Template

Use this when generating a plan-template for a target project's spec-driven pipeline. The generated template is used by the Plan-Implementation skill to produce implementation plans from approved specs.

---

## Template

```markdown
# Implementation Plan: <Feature Title>

**Spec**: specs/<feature-id>/spec.md
**Status**: draft | approved | in-progress | completed

## Architecture Approach

<High-level approach. Which patterns, modules, and layers are involved? Why this approach over alternatives?>

## File Changes

| # | File | Change Type | Description | Symbols | Risk |
|---|------|-------------|-------------|---------|------|
| 1 | <path> | create/modify/delete | <what changes> | <functions/types affected> | low/medium/high |

## Dependency Order

<Which changes must happen first? Group related changes and specify execution order.>

1. **Phase 1**: <Data layer changes> — must complete before Phase 2
2. **Phase 2**: <Business logic changes> — depends on Phase 1
3. **Phase 3**: <UI/API changes> — depends on Phase 2
4. **Phase 4**: <Test additions> — can partially parallelize with Phase 3

## Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| <risk> | low/medium/high | <what breaks> | <how to prevent/recover> |

## Test Strategy

- **Unit tests**: <what to test, which modules>
- **Integration tests**: <cross-module scenarios>
- **UI tests**: <user-facing flows to validate>
- **Manual verification**: <what cannot be automated>

## Rollback Plan

<If this change fails in production or breaks existing behavior, how do we revert? Is the change reversible?>

## Phase -1 Gate Checklist

- [ ] Simplicity: This is the simplest approach that satisfies the requirement
- [ ] Duplication: No existing code can be reused instead
- [ ] Business Logic: All business rules verified against source
- [ ] Impact: Blast radius understood and acceptable
```

---

## Generation Rules

- File changes table must reference real project files from the analyzer
- Dependency order must be correct — the implementor follows this order
- Risk assessment should be honest — do not minimize genuine risks
- The template lives in `.github/templates/<prefix>/` in the generated bundle
- Phase -1 gate checklist connects to the constitution's gates
