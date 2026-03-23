---
name: agent-builder
description: "Use when: creating, refactoring, auditing, upgrading, or debugging Copilot custom agents, skills, instructions, prompts, or hooks for Apple platform software engineering and agile delivery workflows."
---

# Agent Builder

Primary consumer: `Agent Builder` in `.github/agents/agent-builder.agent.md`.

Use this skill to design and generate high-quality Copilot customization bundles for Apple platform projects. This is the single source of truth for the complete kit workflow, bundle shapes, and artifact behavioral requirements.

## Use Cases

- Create a new custom agent for Apple engineering or agile delivery
- Refactor or upgrade existing agent bundles
- Audit weak descriptions, frontmatter, or scopes
- Verify and improve an existing agent ecosystem
- Diagnose invocation failures for skills, instructions, prompts, or agents

## Apple Scope

iOS, iPadOS, macOS, visionOS, watchOS, and tvOS.

**Fallback defaults** (used only when the analyzer cannot determine the project's actual stack): Swift 6, strict concurrency, SwiftUI-first, UIKit/AppKit fallback when required, Swift Testing + XCTest UI tests.

**Alignment rule**: The analyzer's codebase findings always override these fallback defaults. If the project uses Swift 5.9, actor isolation only where explicitly adopted, UIKit-primary architecture, or XCTest without Swift Testing — generated agents must align to the project's actual technology profile, not the latest available.

## Default Behavior

- Discovery Mode when unclear inputs could change architecture; Fast Mode when goals are explicit
- If the user chooses the wrong primitive, offer 2-3 better options
- Generate files in English by default
- Prefer portable designs unless the user asks for repo-locked behavior
- Multi-agent architecture when quality depends on distinct phases
- Hard audit gate for non-trivial bundles
- Refresh official Copilot documentation before any generation flow
- Align generated agents to the project's actual technology profile — never assume latest defaults when the codebase shows otherwise
- When invoked directly: confidence ≥ 95% before generation
- When invoked via prompt: skip confidence interview, analyze directly

## Workflow

### Standard User-Facing Phases

1. Refresh Copilot documentation
2. Analyze the codebase
3. Assess the project and choose bundle architecture
4. Ask targeted follow-up only if needed (auto-skip when possible)
5. Generate or update the bundle
6. Audit and iterate until PASS
7. Deliver the final result

Internal setup work (reading skill files, loading templates, scanning existing files) stays inside these phases, not as extra top-level steps.

### 0. Refresh Official Copilot Guidance

Before analysis or generation:
- Use `Apple Copilot Docs Refresher` to fetch the Mandatory Source Set from `.github/templates/agent-builder/copilot-doc-source-registry.md`
- Verify every mandatory URL still resolves; repair broken links before continuing
- Add Optional sources when the workflow touches skills, memory, MCP, or environment setup
- Produce an in-session refresh brief using `.github/templates/agent-builder/copilot-doc-refresh-brief-template.md`
- Do not persist the brief as a file in the target project
- When maintaining the kit, update `kit-doc-refresh.md`

### 1. Clarify the Outcome

Collect only facts that materially change architecture: target users, tasks, inputs/outputs, domain, autonomy level, tool restrictions, quality bar, Apple platforms, framework constraints, delivery roles.

### 2. Model the Workflow

Define: primary role, boundaries, non-goals, decision points, failure modes, artifacts. For Apple bundles: platform coverage, concurrency/lifecycle/testing expectations grounded in the project's actual technology profile from the analyzer — not kit fallback defaults.

### 3. Choose Primitives

Apply `.github/templates/agent-builder/primitive-decision-matrix.md`. Quick reference:

| Need | Primitive |
|------|-----------|
| Dedicated persona, context isolation | agent |
| Reusable repeatable workflow | skill |
| Always-on standards for matching files | instruction |
| Compact user-facing entry point | prompt |
| Deterministic enforcement | hook (validate with `hook-checklist.md` first) |

For linting tools: prefer instructions + agent validation steps over hooks. Add a hook only when: (1) team wants deterministic blocking, (2) tool reliably available everywhere, (3) false positives rare. See `hook-checklist.md`.

### 4. Build the Bundle

Use scaffold templates from `.github/templates/agent-builder/`:

| Artifact | Template |
|----------|----------|
| agent | `agent-template.md` |
| skill | `skill-template.md` |
| instruction | `instruction-template.md` |
| prompt | `prompt-template.md` |
| workflow asset | `workflow-asset-template.md` |

Pick roles from `apple-role-catalog.md`. Replace every `<...>` placeholder with project-grounded content.

**For `/generate-workflow-agents`, default to a full workflow kit:**
- Project-tailored `copilot-instructions.md` (standard filename, not prefixed; do not copy the kit's own)
- Conductor + specialist agents with project-derived file prefix
- Multiple focused skills: delivery workflow (mandatory) + domain-specific (testing, investigation, review)
- Multiple narrow instructions: implementation conventions + test conventions at minimum
- At least 2 prompts: primary delivery + secondary entry point
- At least 2 templates/checklists for recurring hand-offs
- Explicit hook decision
- Bidirectional cross-references: agents reference skills, skills list their agents, templates are referenced by at least one file
- Intermediate working files deleted before finalizing
- Dirty worktree: preserve unrelated edits, stop only for direct path conflicts

**Naming rule**: Use a project-derived file prefix for generated files. Never prefix `copilot-instructions.md`.

### 5. Validate Before Finalizing

Run `.github/templates/agent-builder/agent-audit-rubric.md`. Key checks:
- Cross-reference integrity (no orphaned templates or skills, bidirectional linking)
- Delivery workflow skill present
- `copilot-instructions.md` uses standard filename, no intermediate files left
- Frontmatter valid with discoverable descriptions
- Roles clear, goals explicit, output contracts defined
- Apple platform scope explicit

### 6. Audit and Revise

Send to auditor. If `REVISE`, patch every finding (major and minor) and re-audit. Do not accept or skip minor findings — all must be resolved before PASS. If `BLOCKED`, ask the missing questions. Repeat until PASS with zero open findings.

## Bundle Shapes

### Lean: narrow single workflow
- 1 prompt + 1 agent or skill

### Standard: meaningful but focused workflow
- 1 agent + 1 skill + 1 prompt + optional instruction + 1 template

### Full Kit: Apple delivery workflows (default for `/generate-workflow-agents`)
- `copilot-instructions.md` for broad project context
- Conductor + 3+ specialist agents
- 2+ skills (delivery + domain-specific)
- 2+ instructions (implementation + testing)
- 2+ prompts (delivery + secondary)
- 2+ templates for recurring hand-offs
- Explicit hook decision

Full specification and minimums table in `apple-quality-auditor.agent.md`.

## Artifact Requirements

### Agent
- Mission, use-when, non-goals, decision rules, output contract
- Collaboration model: inputs consumed, outputs produced, hand-off criteria, auto-return vs escalation conditions
- **All agents**: structured clarification questions (what to ask, when to skip because the codebase answers), anti-patterns list, structured output contract
- **Implementor**: pre-impl checklist (trace existing flow, confirm business rules, check duplication, scope affected files), code reasoning (explain business justification before writing each file), incremental implementation (verify data → business → API in chunks), verify-fix loop (build → targeted tests for touched feature/suites → lint, max 3 retries, stop and report). Verify-fix loop must treat lint warnings as failures when a linter is configured — use `--strict` or equivalent flag. Build/test commands must use the analyzer-detected simulator destination, never a hardcoded device model. Full test-suite runs are conditional, not default: run full suite only when risk is high (shared/core modules touched, broad dependency impact, release/CI gate, or explicit user request)
- **Reviewer**: deep context gathering (full file content + dependency graph + callers, not just diffs), short-circuit on functional blockers before running technical review, severity-driven verdicts with actionable code-level findings
- **Investigator**: structured as-is/to-be analysis with file and line references, to-be change table (component, change type, file, business reason), impact matrix with severity levels, scenario mapping
- **Orchestrator**: intent-based auto-routing to sub-agents so users never manually pick agents, mandatory confirmation checkpoint between investigation and implementation, structured completion report, micro-change lane for low-risk edits, reviewer conflict resolution, inter-agent iteration loops (implement → test → review → back to implement until review passes)

### Skill
- Repeatable workflow with decision criteria and validation checks
- State which agents use it (bidirectional linking mandatory)
- Delivery workflow skill is always mandatory minimum for full kits
- Create domain-specific skills (testing, investigation, review) when they serve multiple agents
- Encode specific workflow steps, decision trees, and validation checklists — not generic advice

### Instruction
- Narrow `applyTo` scope, single convention domain
- Separate files for distinct domains (implementation, testing, UI, data layer)
- Avoid duplicating the entire skill body

### Prompt
- Short, discoverable, route to the right agent
- At least 2: primary delivery + secondary entry point
- Make the default workflow lane obvious

### Hook
- Only when deterministic enforcement is truly needed and safe
- Validate against `hook-checklist.md` first

## Anti-Patterns

- Vague descriptions
- `applyTo: "**"` without strong reason
- Skills that are only generic advice
- Agents duplicating prompt/instruction content
- Hooks for convenience rather than safety
- Under-building a workflow kit when fuller coverage would help
- Apple-focused bundle that reads like generic mobile guidance
- Assuming latest Swift/concurrency/UI defaults when the project uses an older or different stack
- Merging analysis, generation, and audit when each has distinct failure modes

## Final Output Contract

- Chosen architecture and why
- Created or changed files
- Validation results
- Remaining risks
- Audit outcome
