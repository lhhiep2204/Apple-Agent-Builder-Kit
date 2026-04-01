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
- Generated agents must not declare `tools` or `mcp-servers` in frontmatter by default; omitting these fields keeps all tools (including user-configured MCP servers) accessible
- Generated agents must include explicit MCP tool preference guidance for any agent that may interact with external services — "do not restrict MCP in frontmatter" must not be misread as "do not use MCP at runtime"
- Align generated agents to the project's actual technology profile — never assume latest defaults when the codebase shows otherwise
- When invoked directly: confidence ≥ 95% before generation
- When invoked via prompt: skip confidence interview, analyze directly

## Single-Session Completion Rule

The entire bundle must be generated within a single continuous session. Never abandon the workflow or leave generation incomplete.

- **Start to finish in one session**: Once generation begins, continue through analyze → generate → audit → revise → deliver without stopping.
- **Interactive clarification when blocked**: When user input is genuinely needed (high-risk ambiguity that would materially change the bundle), present a structured question with 2-4 options and a recommended default. Wait for the user's response, then resume processing immediately.
- **Low-risk ambiguity**: Continue with an explicit provisional assumption. State the assumption so the user can override.
- **Never abandon**: Do not end a response mid-workflow claiming "we can continue later" or "let me know when you're ready." If blocked on user input, ask the question and explicitly commit to continuing after receiving the answer.
- **Audit failures are not session-enders**: When the auditor returns `REVISE`, fix every finding and re-audit in the same session. When `BLOCKED`, ask the missing questions immediately, wait for answers, then resume.
- **Error recovery**: If a tool fails or an approach is blocked, try alternatives within the same session. Escalate to the user only when no automated recovery is possible.

This rule applies to the kit itself (Agent Builder generating bundles) and to generated orchestrator agents (which must also complete delegated workflows in one session).

## Workflow

### Standard User-Facing Phases

1. Analyze the codebase
2. Assess the project and choose bundle architecture
3. Ask targeted follow-up only if needed (auto-skip when possible)
4. Generate or update the bundle
5. Audit and iterate until PASS
6. Deliver the final result

Internal setup work (reading skill files, loading templates, scanning existing files) stays inside these phases, not as extra top-level steps.

### Kit Reference Files

The kit maintains one persistent reference file that the generator reads directly during generation:

- `.github/templates/agent-builder/kit-doc-refresh.md` — Copilot product behavior: frontmatter rules, primitives, tool behavior, hook behavior, instruction precedence. Updated by `Apple Copilot Docs Refresher` during kit maintenance.

Apple platform domain knowledge is not maintained as a separate snapshot. It must come from the target project's codebase analysis: source files, project config, tests, resources, entitlements, and migration signals. When project signal is thin, use cautious fallback assumptions and mark them explicitly.

The docs snapshot is an authoritative generator input. It does not need to be fetched each run — the kit owner updates it periodically using the refresher agent during kit maintenance sessions.

### 1. Clarify the Outcome

Collect only facts that materially change architecture: target users, tasks, inputs/outputs, domain, autonomy level, tool restrictions, quality bar, Apple platforms, framework constraints, delivery roles.

Clarification protocol:
- **Low-risk ambiguity**: Continue with an explicit provisional assumption. State it clearly so the user can override.
- **High-risk ambiguity** (would materially change architecture): Present a structured choice block: 2-4 options, one recommended default, plus an "Other" free-input option. Wait for user response, then continue.
- Never stop the workflow entirely after asking a question. Either continue provisionally (low-risk) or wait for the response and resume (high-risk).

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
- `copilot-instructions.md` — workspace-level instructions automatically loaded by Copilot for all interactions. If the target project already has one, update it to integrate with the generated agent ecosystem while preserving existing relevant content. If the target project does not have one, create it. Content: concise project overview, technology stack, core conventions, agent ecosystem guide (available agents, primary prompt entry points, how to invoke), and cross-references to detailed instructions.
- Project context instruction (`<prefix>-project-context.instructions.md` with `applyTo: "**"`) for detailed project facts (architecture, domain glossary, extended conventions, bundle evolution guidance). Supplements `copilot-instructions.md` with depth that would be too verbose for the workspace-level file.
- Conductor + specialist agents with project-derived file prefix
- Multiple focused skills: delivery workflow (mandatory) + domain-specific (testing, investigation, review)
- Multiple narrow instructions: implementation conventions + test conventions at minimum
- Optional business-domain assets when project complexity warrants them (domain map, domain-scoped instructions, or a reusable business-rules workflow)
- At least 2 prompts: primary delivery + secondary entry point
- At least 2 templates/checklists for recurring hand-offs
- Explicit hook decision
- Bidirectional cross-references: agents reference skills, skills list their agents, templates are referenced by at least one file
- Intermediate working files deleted before finalizing
- Dirty worktree: preserve unrelated edits, stop only for direct path conflicts

**Naming rule**: Use a project-derived file prefix for generated files.

### 5. Validate Before Finalizing

Run `.github/templates/agent-builder/agent-audit-rubric.md`. Key checks:
- Cross-reference integrity (no orphaned templates or skills, bidirectional linking)
- Delivery workflow skill present
- `copilot-instructions.md` present (created or updated) with workspace-level content
- Project context instruction file present, no intermediate files left
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
- `copilot-instructions.md` — workspace-level instructions (created or updated)
- Project context instruction (`<prefix>-project-context.instructions.md`, `applyTo: "**"`) for detailed project facts
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
- **All agents**: structured clarification questions (what to ask, when to skip because the codebase answers), anti-patterns list, structured output contract, explicit MCP tool preference for external service access (issue trackers, project management, documentation platforms)
- **Implementor**: pre-impl checklist (trace existing flow, confirm business rules, check duplication, scope affected files), code reasoning (explain business justification before writing each file), incremental implementation (verify data → business → API in chunks), verify-fix loop (build → targeted tests for touched feature/suites → lint, max 3 retries, stop and report). Verify-fix loop must treat lint warnings as failures when a linter is configured — use `--strict` or equivalent flag. Build/test commands must use the analyzer-detected simulator destination, never a hardcoded device model. Full test-suite runs are conditional, not default: run full suite only when risk is high (shared/core modules touched, broad dependency impact, release/CI gate, or explicit user request)
- **Code Reviewer**: always runs thorough combined technical + functional review. Technical scope: architecture, concurrency, performance, security, dependency graph analysis. Functional scope: acceptance criteria, domain rules, edge cases, offline behavior. Workflow: short-circuit on functional blockers before running full technical review; deep context gathering (full file content + dependency graph + callers, not just diffs); severity-driven verdicts with actionable code-level findings. Supports PR review (diff-based, approval/request-changes verdict) and general code change review. Output: structured review report with severity levels (blocker/major/minor/suggestion), clear pass/revise/blocked verdict, and revision checklist when revise
- **Investigator**: structured as-is/to-be analysis with file and line references, to-be change table (component, change type, file, business reason), impact matrix with severity levels, scenario mapping
- **Business-heavy bundles**: when a business domain registry, domain map, domain-scoped instructions, or business-domain skill exists, Business Analyst and Investigator maintain or refresh it; Implementor and Code Reviewer must consult it before changing or approving business logic; Dev Orchestrator includes it in hand-off requirements.
- **Orchestrator**: intent-based auto-routing to sub-agents so users never manually pick agents, mandatory confirmation checkpoint between investigation and implementation, structured completion report, micro-change lane for low-risk edits, reviewer conflict resolution, inter-agent iteration loops (implement → test → review → back to implement until review passes)
- **Orchestrator single-session rule**: the orchestrator must complete the entire delegated workflow within one session. Never abandon mid-workflow. When user input is needed, present structured options and wait for response, then resume. When a sub-agent fails or returns REVISE, fix and retry in the same session.
- **Orchestrator clarification behavior**: never end the workflow after a clarification prompt alone; provide a decision menu, recommended default, and continue execution on a provisional track until user override

### YAML Frontmatter Generation
- `agents` field MUST use inline JSON-style array syntax with exact agent display names: `agents: ["Name A", "Name B"]` — NEVER use block list syntax (`- item`) and NEVER use filenames
- `description` field MUST be a double-quoted string with concrete trigger phrases
- `name` field MUST be an unquoted string matching the intended display name
- Prompt files MUST use `agent` field (not `mode`) for routing to a specific agent: `agent: "Agent Display Name"`
- Only emit frontmatter keys documented in current official Copilot documentation. For agents: `name`, `description`, `agents`, `tools`, `model`, `target`, `user-invocable`, `disable-model-invocation`, `mcp-servers`, `handoffs`, `hooks`. For prompts: `description`, `agent`.
- Do not emit `tools` or `mcp-servers` unless explicitly requested
- All generated files in the same bundle MUST use identical YAML formatting conventions

### Skill
- Repeatable workflow with decision criteria and validation checks
- State which agents use it (bidirectional linking mandatory)
- Delivery workflow skill is always mandatory minimum for full kits
- Create domain-specific skills (testing, investigation, review) when they serve multiple agents
- Encode specific workflow steps, decision trees, and validation checklists — not generic advice

**Domain knowledge placement decision** — when the analyzer finds Apple domain knowledge for a technology area (SwiftUI, UIKit, persistence, concurrency, testing, accessibility, security, capabilities, etc.), place it as follows:

| Signal | Where to put the knowledge |
|--------|---------------------------|
| Domain is heavily used in the project AND multiple agents need it (e.g., implementor + reviewer both need SwiftUI patterns) | Create a dedicated domain skill |
| Domain is used in the project but only one agent needs it | Embed directly in that agent's instructions |
| Domain knowledge is a small set of conventions (< 10 rules) | Embed in the relevant instruction file (`applyTo` the matching path pattern) |
| Domain is lightly used or the project barely touches it | Skip — do not create a skill or bloat instructions for thin signal |
| Domain signal is thin or unknown | Prefer omission or an explicit uncertainty note over prescriptive guidance |

Do not generate one skill per detected Apple technology area. Create a domain skill only when it earns its place through multi-agent reuse and strong project signal from the analyzer.

### Business Knowledge Persistence
- Always capture the minimum shared business context in the project context instruction: domain glossary, key business rules, lifecycle states, and invariants.
- For projects with richer business complexity, choose the lightest primitive that preserves shared understanding:
	- project context instruction only: small apps or products whose business rules fit in a concise summary
	- domain-scoped instructions: business rules that map cleanly to specific paths or modules
	- reusable workflow asset or domain map: multiple domains, lifecycle-heavy workflows, or cross-domain dependencies that several agents must reference
	- business-domain skill: only when multiple agents reuse the same repeatable business-analysis workflow or interpretation process
- If a bundle includes a business domain registry, domain map, domain-scoped instructions, or a business-domain skill, Business Analyst, Investigator, Implementor, Code Reviewer, and Dev Orchestrator must reference it explicitly in their collaboration inputs or decision rules.
- Do not generate a dedicated business-domain artifact when the project's business context fits cleanly in the project context instruction and the normal planning/investigation assets.
- Do not add a dedicated kit subagent for business-domain modeling by default. The analyzer extracts business context, and the generator decides how to persist it. Add an extra specialization only when business-domain synthesis becomes a distinct workflow with materially different failure modes.

### Instruction
- Narrow `applyTo` scope, single convention domain
- Separate files for distinct domains (implementation, testing, UI, data layer)
- Avoid duplicating the entire skill body

### Prompt
- Short, discoverable, route to the right agent
- At least 2: primary delivery + secondary entry point
- Make the default workflow lane obvious
- Frontmatter MUST use `agent` field (not `mode`) for routing to a specific agent

### Hook
- Only when deterministic enforcement is truly needed and safe
- Validate against `hook-checklist.md` first

## Generation Principles

Non-negotiable principles every generated agent must encode. These are the behavioral floor for all generated agents, regardless of role.

1. **Understand before changing** — Trace existing code paths, data flow, and callers before modifying anything. Never assume; verify.
2. **Confirm business logic** — Identify business rules before writing code. Ask when rules are ambiguous, not after the implementation is wrong.
3. **No duplicate validation across layers** — Check for existing validation before adding new checks. Each rule enforced in exactly one layer.
4. **Respect module boundaries** — Do not reach across architectural boundaries. Use established interfaces and dependency patterns.
5. **Clarify before acting on ambiguity** — When intent or requirements are unclear, ask with options and a recommended default. Never guess silently.
6. **Verify before claiming completion** — Run the project's actual build, test, and lint commands. Do not claim success without evidence.
7. **Prefer simplicity** — Choose the simplest correct solution. Avoid premature abstraction, unnecessary indirection, or speculative generalization.
8. **Explain decisions and report outcomes** — State what was done, why, and what changed. Include evidence of verification in completion reports.

The generator must embed these principles in every generated agent's instructions. The auditor must verify their presence.

## Context Optimization Rules

Rules for efficient context usage in generated agents and artifacts.

### Signal-to-Noise Discipline
- Generated agents should produce concise, phase-bounded outputs — not restate upstream context
- Hand-offs between agents prefer delta summaries with file and line references over repeated full-context prose
- Static rules belong in instructions and skills (loaded once), not duplicated across every agent's instructions

### Three-Layer Context Model
Generated bundles should distribute context efficiently across three layers (plus a workspace-level foundation):

**Foundation**: `copilot-instructions.md` — Workspace-level instructions automatically loaded by Copilot for all interactions. Concise project overview, technology stack, core conventions, agent ecosystem guide (available agents, prompt entry points, how to invoke). Target 40-80 lines. This is the entry point for anyone using Copilot in the project.

1. **Project context instruction** (`<prefix>-project-context.instructions.md`, `applyTo: "**"`) — Detailed project facts: architecture, extended conventions, domain glossary, bundle evolution guidance. Supplements `copilot-instructions.md` with depth. Keep concise but thorough.
2. **Path-scoped instructions** — Narrow, file-pattern-scoped rules. Loaded only when matching files are active. Keep single-purpose.
3. **Agent instructions** — Role-specific workflows, decision rules, and output contracts. Loaded only when the agent is invoked. Do not repeat what instructions or skills already cover.

Do not duplicate content between `copilot-instructions.md` and the project context instruction — the workspace-level file is the brief overview and agent guide, the project context instruction is the deep reference.

### Context Budget Guidance for Generated Project Context Instruction
- Target length: 80-150 lines for typical projects. Exceed only when genuine complexity demands it.
- Include: project overview, architecture summary, technology stack, key conventions, domain glossary, quality expectations.
- Exclude: role-specific workflows (belongs in agents), file-pattern rules (belongs in path-scoped instructions), repeatable processes (belongs in skills).
- Every line must earn its place — if a fact only matters for one role, put it in that agent's instructions instead.

## Bundle Evolution Guidance

The generated project context instruction should include a brief section guiding post-generation maintenance:

- **When to update agents**: After major architecture changes, new module additions, framework migrations, or when agents produce consistently wrong outputs.
- **When to promote patterns**: If you find yourself correcting the same agent behavior repeatedly, extract the correction into an instruction file with a narrow `applyTo` scope.
- **When to add instructions**: When a new convention emerges that applies to specific file patterns (e.g., new test naming convention, new module boundary rule).
- **Drift signals**: Agents referencing removed files, using outdated API patterns, or suggesting conventions the team abandoned are signs of drift — update the affected agents.

The generator must include this guidance section in every generated project context instruction.

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
