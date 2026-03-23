# Agent Audit Rubric

Quick-reference checklist before finalizing. Full scoring standard and minimums table in `apple-quality-auditor.agent.md`.

## Verdicts

- `PASS`: every finding (major and minor) is resolved. Zero open issues.
- `REVISE`: any finding still open, regardless of severity. All must be fixed before PASS.
- `BLOCKED`: missing information prevents credible validation.

## Full Workflow Kit Minimums

`copilot-instructions.md` (1) + conductor (1) + 3+ specialists + 2+ skills (delivery + domain) + 2+ instructions (impl + test) + 2+ prompts + 2+ templates + hook decision (1). Below these = Standard or Lean.

## Quick Checks

1. **Discovery**: descriptions concrete, keyword-rich, likely to match real user requests?
2. **Architecture**: each artifact necessary? Conductor/specialist split justified? Bundle complete for workflow?
3. **Execution**: constraints explicit? Output contracts clear? Behavioral patterns role-appropriate per SKILL.md? Collaboration lanes defined with hand-offs and iteration loops? Validation guidance repo-grounded? Build/test commands use project-derived simulator destinations (not hardcoded device names)? Verify-fix loop includes lint with `--strict` when linter is configured? Test strategy defaults to targeted tests and escalates to full-suite only under explicit risk/release criteria?
4. **Apple Specificity**: platforms, frameworks, concurrency, testing, accessibility explicit? Generated agents aligned to the project's actual technology profile, not kit fallback defaults?
5. **Scope Discipline**: files single-purpose? `applyTo` narrow? No drift risk?
6. **Ecosystem Coherence**: no naming conflicts? No role overlap? Existing agents evaluated?

## Cross-Reference Integrity

- Every template referenced by ≥1 agent/skill/prompt (no orphans)
- Every skill referenced by ≥1 agent with bidirectional linking (no orphans)
- Delivery workflow skill present (mandatory for full kits)
- No intermediate files left in target project
- `copilot-instructions.md` uses standard filename (not prefixed)
- Kit's own `copilot-instructions.md` excluded from target project

## Pass Rule

Do not pass merely readable bundles. Do not pass with "accepted" minor issues — every finding must be resolved. Pass only when all findings are fixed, the bundle is specific, discoverable, likely to perform well, and cross-reference integrity is clean. Residual risks must be genuine uncertainties (e.g., future platform changes), never fixable weaknesses.
