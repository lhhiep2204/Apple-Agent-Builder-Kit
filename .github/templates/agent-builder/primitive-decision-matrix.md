# Primitive Decision Matrix

Use this when deciding what to generate.

| Need | Generate | Avoid When |
|------|----------|------------|
| dedicated persona with context isolation | agent | a skill alone can do the job cleanly |
| reusable repeatable workflow with assets | skill | the workflow is a one-off prompt |
| always-on standards for matching files | instruction | the guidance is situational or too broad |
| compact user-facing entry point | prompt | the agent or skill is already obvious and discoverable |
| deterministic enforcement | hook | an instruction is sufficient or the command is risky |

## Apple Split Rule

Use conductor plus specialist subagents when quality depends on three distinct questions:
- what the Apple project really is
- what bundle should be generated
- whether the generated bundle is actually strong enough

If those questions can be answered credibly in one narrow workflow, a single specialist may be better.