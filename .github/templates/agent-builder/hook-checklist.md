# Hook Checklist

Use this before adding any hook.

## Add A Hook Only If

- an instruction cannot enforce the behavior reliably
- the command is deterministic and safe
- the failure mode is costly enough to justify enforcement
- the team accepts the automation and its side effects

## Validate Before Shipping

- the hook command is auditable
- the command does not destroy user work
- the hook does not create noisy false positives
- the hook will not block normal workflows unnecessarily
- the hook behavior is documented in the related agent, skill, or guide

## Common Mistakes

- using hooks for advisory guidance
- using hooks without clear ownership
- blocking tools for convenience rather than safety
- adding shell logic that is too brittle to maintain

## Linting Tools (SwiftLint, SwiftFormat, etc.)

Linting tools are a common hook candidate but are usually better handled without hooks:

**Prefer instructions + agent validation steps when:**
- the lint tool may not be installed in all environments (CI, Copilot sandbox, teammates)
- lint rules are advisory and violations should be fixed, not blocked
- the implementor and test-specialist can run lint as a post-generation validation step
- you want portable behavior across GitHub.com and VS Code

**Consider a hook only when:**
- the team explicitly wants deterministic blocking on lint violations
- the lint command is reliably available in every environment the agent runs in
- false positives are rare and well-understood
- the hook timeout (30s default) is sufficient for the lint run

**Recommended default approach:**
1. Encode lint rules and lint command in the implementation instruction
2. Require implementor and test-specialist to run lint after code generation
3. Include lint check in the orchestrator's validation gate
4. Document the no-hook rationale in the hook decision artifact