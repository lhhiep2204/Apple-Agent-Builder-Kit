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
- **Interactive clarification when blocked**: When user input is genuinely needed (high-risk ambiguity that would materially change the bundle), use `vscode_askQuestions` to present a structured question with 2-4 options and a recommended default. This tool keeps the session alive — never write questions as plain text that ends the response. Wait for the user's response, then resume processing immediately.
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

### Community Skill Discovery (Runtime, MCP Conditional)

During the analysis phase, when MCP GitHub tools are available, the analyzer reads the community skill directory at `twostraws/Swift-Agent-Skills` to discover relevant domain knowledge for the target project's tech stack.

- **Trigger**: MCP GitHub tools accessible at runtime (e.g., `mcp_github_get_file_contents`)
- **Timing**: After the analyzer determines the Technology Alignment Profile, as part of the analysis phase
- **Process**: Read the directory README, match categories to project tech stack, follow links to individual skill repos for matching categories, extract actionable domain knowledge
- **Output**: Community Skill Discovery Results section in the analysis output — matched categories, extracted knowledge, and recommendations for the generator
- **Generator consumption**: Embed core knowledge in generated agents + recommend users install community skills for latest updates
- **Precedence**: Project-specific code patterns always override community skill guidance when they conflict
- **Graceful skip**: When MCP GitHub is not available, the analyzer notes the skip and generation proceeds using only project evidence and kit knowledge — no degradation in core functionality

### 1. Clarify the Outcome

Collect only facts that materially change architecture: target users, tasks, inputs/outputs, domain, autonomy level, tool restrictions, quality bar, Apple platforms, framework constraints, delivery roles.

Clarification protocol:
- **Low-risk ambiguity**: Continue with an explicit provisional assumption. State it clearly so the user can override.
- **High-risk ambiguity** (would materially change architecture): Use `vscode_askQuestions` to present a structured choice block: 2-4 options, one recommended default, plus an "Other" free-input option. Wait for user response, then continue.
- **Tool requirement**: Always use `vscode_askQuestions` for structured clarification — never write questions as plain response text. Plain-text questions end the session; `vscode_askQuestions` keeps it alive.
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
- At least 3 prompts: primary delivery + spec entry + secondary
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

### Single: narrow single workflow
- 1 prompt + 1 agent or skill

### Focused: meaningful but focused workflow
- 1 agent + 1 skill + 1 prompt + optional instruction + 1 template

### Full Kit: Apple delivery workflows (default for `/generate-workflow-agents`)
- `copilot-instructions.md` — workspace-level instructions (created or updated)
- Project context instruction (`<prefix>-project-context.instructions.md`, `applyTo: "**"`) for detailed project facts
- Constitution (`<prefix>-constitution.md`) — immutable governance rules and Phase -1 gates
- Conductor + 4+ specialist agents (implementation, testing, review pipeline, investigation)
- Review pipeline: Code Review orchestrator + Functional Reviewer + Technical Reviewer + Platform Reviewer (Apple-specific)
- 3+ skills (delivery workflow + spec-driven pipeline + domain-specific)
- 2+ instructions (implementation + testing)
- 3+ prompts (primary delivery + spec entry + secondary)
- 3+ templates for recurring hand-offs (including spec/plan/tasks)
- Essential docs: user-playbook, review-playbook
- Explicit hook decision (SwiftLint/SwiftFormat auto-format, xcodebuild compile-check when warranted)
- Drift detection guidance in bundle evolution section
- Review memory promotion guidance in orchestrator

Full specification and minimums table in `apple-quality-auditor.agent.md`.

Every project receives the full bundle. Small projects benefit from the same agent capabilities as large ones — the agents adapt their behavior to context, but no agent roles, governance artifacts, or pipeline features are withheld based on project size.

## Artifact Requirements

### Agent
- Mission, use-when, non-goals, decision rules, output contract
- Collaboration model: inputs consumed, outputs produced, hand-off criteria, auto-return vs escalation conditions
- **All agents**: structured clarification questions using `vscode_askQuestions` (what to ask, when to skip because the codebase answers), anti-patterns list, structured output contract, explicit MCP tool preference for external service access (issue trackers, project management, documentation platforms). Clarification must always use `vscode_askQuestions` — never plain-text questions that end the session. Evidence standard: every business-rule claim must be backed by code/doc evidence, user confirmation, or labeled `[ASSUMPTION]` / `[NEEDS CLARIFICATION]`.
- **All agents with handoffs**: include `handoffs:` in YAML frontmatter for explicit delegation targets with label and prompt. Example: `handoffs: [{agent: "Test Specialist", label: "Generate Tests", prompt: "Generate tests for..."}]`. This replaces implicit delegation through prose instructions.
- **Implementor**: pre-impl checklist (trace existing flow, confirm business rules, check duplication, scope affected files), code reasoning (explain business justification before writing each file), incremental implementation (verify data → business → API in chunks), verify-fix loop (build → targeted tests for touched feature/suites → lint, max 3 retries, stop and report). Verify-fix loop must treat lint warnings as failures when a linter is configured — use `--strict` or equivalent flag. Build/test commands must use the analyzer-detected simulator destination, never a hardcoded device model. Full test-suite runs are conditional, not default: run full suite only when risk is high (shared/core modules touched, broad dependency impact, release/CI gate, or explicit user request). For large tasks: follow the Large Task Execution Pattern (decompose → persist plan → chunked execution → intermediate validation → context management → completion). Phase -1 constitutional gates (simplicity, duplication, business logic, impact) must pass before implementation begins.
- **Code Review Orchestrator**: orchestrates a multi-stage review pipeline with short-circuit on blockers. Delegates to Functional Reviewer, Technical Reviewer, and Platform Reviewer (Apple-specific) sub-agents. Stage 0: context gathering (full file content + dependency graph + callers). Stage 1: Functional Review — business logic correctness, AC traceability, edge cases. If functional BLOCKER found, REJECT immediately without proceeding to technical review. Stage 2: Technical Review — architecture, concurrency, performance, security, layer responsibility, migration safety. Stage 3 (conditional): Platform Review — Apple-specific checks (memory management, UI thread, SwiftUI recomposition, actor isolation, accessibility). Combined report with severity-driven verdicts.
- **Functional Reviewer**: validates code changes exclusively from business correctness perspective. AC ↔ Test ↔ Code traceability mapping. Adversarial edge-case analysis. Cross-domain data integrity checks. Evidence standard: every claim backed by code/doc anchor, user confirmation, or labeled `[ASSUMPTION]`/`[NEEDS CLARIFICATION]`. Business context confidence level (High/Medium/Low). Short-circuit: any BLOCKER stops the review pipeline.
- **Technical Reviewer**: validates code changes from technical quality perspective only. API backward compatibility checks. Database/migration safety analysis. Domain boundary violations (module boundaries, layer responsibility). NFR compliance (logging, error handling, external call protection). Performance review. Does NOT verify business requirements — that is the Functional Reviewer's job.
- **Platform Reviewer (Apple-specific)**: validates Apple platform concerns not covered by generic technical review. Memory management (retain cycles, actor isolation). UI thread safety (MainActor boundaries, background task dispatching). SwiftUI recomposition analysis. Concurrency correctness (Sendable, task cancellation). Accessibility and localization completeness. Platform capability usage (entitlements, privacy). Triggered only when changed files include Swift/SwiftUI/UIKit code.
- **Investigator**: structured as-is/to-be analysis with file and line references producing a **repository impact map**: concrete file list with change descriptions, real symbol names and existing patterns to follow, dependency-ordered change groups. To-be change table (component, change type, file, business reason), impact matrix with severity levels, scenario mapping. Output must be optimized for agent legibility — structured tables and lists, not narrative prose.
- **Business-heavy bundles**: when a business domain registry, domain map, domain-scoped instructions, or business-domain skill exists, Business Analyst and Investigator maintain or refresh it; Implementor and Code Reviewer must consult it before changing or approving business logic; Dev Orchestrator includes it in hand-off requirements.
- **Orchestrator**: intent-based auto-routing to sub-agents so users never manually pick agents, mandatory confirmation checkpoint between investigation and implementation, structured completion report, micro-change lane for low-risk edits, reviewer conflict resolution, inter-agent iteration loops (implement → test → review → back to implement until review passes)
- **Orchestrator planning lane**: for complex tasks (large migrations, multi-module refactors, cross-cutting changes), the orchestrator must activate a planning lane before delegation: decompose the task into ordered phases, persist the plan to session memory, track progress via `manage_todo_list`, and execute phases sequentially with intermediate validation. The planning lane replaces the default direct-delegation path when task complexity exceeds the threshold (>10 files, >3 modules, or migration scope).
- **Orchestrator single-session rule**: the orchestrator must complete the entire delegated workflow within one session. Never abandon mid-workflow. When user input is needed, use `vscode_askQuestions` to present structured options and wait for response, then resume. When a sub-agent fails or returns REVISE, fix and retry in the same session.
- **Orchestrator clarification behavior**: never end the workflow after a clarification prompt alone; use `vscode_askQuestions` to provide a decision menu with recommended default, and continue execution on a provisional track until user override
- **Orchestrator harness awareness**: the orchestrator should track recurring failure patterns across tasks and flag when the harness (instructions, conventions, linter rules) should be improved rather than just fixing individual outputs. Include a "harness improvement suggestions" section in completion reports when patterns are detected.
- **Orchestrator context management**: for long tasks, the orchestrator must persist progress to session memory after each major phase, use `manage_todo_list` for visible tracking, and compact context when conversation grows large. Read progress notes at the start of each phase to re-ground.

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
5. **Clarify before acting on ambiguity** — When intent or requirements are unclear, use `vscode_askQuestions` to present structured options with a recommended default. Never guess silently, and never write questions as plain text that ends the session — `vscode_askQuestions` keeps the workflow running.
6. **Verify before claiming completion** — Run the project's actual build, test, and lint commands. Do not claim success without evidence.
7. **Prefer simplicity** — Choose the simplest correct solution. Avoid premature abstraction, unnecessary indirection, or speculative generalization.
8. **Explain decisions and report outcomes** — State what was done, why, and what changed. Include evidence of verification in completion reports.

The generator must embed these principles in every generated agent's instructions. The auditor must verify their presence.

## Harness Engineering Alignment

Generated agents must follow harness engineering principles — designing the environment for reliable agent execution, not just writing instructions.

### Feedforward and Feedback Controls

Every generated agent ecosystem should distinguish two control types:

- **Feedforward (Guides)**: Context provided *before* the agent acts to increase the probability of correct first output. Examples: architecture docs, coding conventions in instructions, implementation notes with real file paths and symbol names, structured task templates.
- **Feedback (Sensors)**: Checks that run *after* the agent acts to enable self-correction. Examples: build commands, test suites, linters, type checkers, AI code review.

Within each type, distinguish:
- **Computational controls**: Deterministic, fast, reliable — linters, type checkers, tests, structural analysis. Run on every change.
- **Inferential controls**: Semantic, slower, non-deterministic — AI code review, LLM-as-judge. Run selectively (e.g., PR review, complex changes).

Generated verify-fix loops are feedback sensors. Generated instructions and skills are feedforward guides. The analyzer should assess the project's existing computational and inferential controls so generated agents leverage them.

### Structured Investigation Output (Repository Impact Map)

The Investigator's output must function as a **repository impact map** — grounded in actual code, not abstract analysis:
- List every file to be modified with specific change descriptions
- Reference real symbol names, function signatures, and existing patterns to follow
- Group changes by module/domain with dependency ordering
- Include human confirmation checkpoint before implementation begins
- The Orchestrator must not proceed to implementation until the impact map is reviewed

### Execution Plans as Persistent Artifacts

For complex or multi-session work, generated agents must persist execution plans as files rather than keeping them only in-session:
- Use session memory files (`/memories/session/`) for investigation findings, migration plans, and progress logs
- Execution plans should include: goal, current phase, completed steps, remaining steps, blocked items, key decisions made
- Plans are first-class artifacts — versioned, structured, and co-located with the work
- Agents must read existing plans at the start of work and update them as progress is made

### Agent Legibility

Generated agent outputs must be optimized for downstream consumption by other agents (not just humans):
- Investigation reports must be structured with clear sections, file references, and machine-parseable change tables — not narrative prose
- Hand-off documents must use consistent formats that downstream agents can parse reliably
- Completion reports must include structured evidence (commands run, results, files changed) not just summary text
- Prefer tabular and list formats over paragraph-form explanations for inter-agent communication

### Steering Loop

Generated agent ecosystems should include guidance for improving the harness over time:
- When an issue recurs across multiple tasks, the fix should target the harness (instructions, conventions, linter rules) — not just the output
- Generated bundle evolution guidance must include: "When you find yourself correcting the same agent behavior repeatedly, extract the correction into an instruction file or promote it into a linting rule"
- The orchestrator should track recurring failure patterns and flag them for harness improvement

### Entropy Management

Generated orchestrator agents should include entropy management for long-lived codebases:
- Periodic drift detection: agents referencing removed files, outdated conventions, or abandoned patterns
- Recurring cleanup patterns: identify dead code, stale docs, inconsistent naming
- Quality tracking: flag when patterns degrade across multiple changes
- The orchestrator's completion report should include a brief "codebase health" section noting any drift signals observed

## Large Task Execution Pattern

For complex, multi-step tasks (large migrations, major refactors, cross-module changes), generated orchestrator and implementor agents must follow this pattern:

### 1. Decomposition
- Break the task into ordered phases based on dependency analysis
- Each phase must be independently verifiable (build + tests pass after each phase)
- Identify phase dependencies — which phases must complete before others can start
- Estimate scope per phase (files, modules, risk level)

### 2. Planning Persistence
- Write the decomposed plan to a session memory file before starting execution
- Plan format: phase list with status (not-started / in-progress / completed / blocked), file targets, validation criteria
- Use `manage_todo_list` to track phases as visible progress indicators

### 3. Chunked Execution
- Execute one phase at a time — never attempt the entire migration in a single pass
- After each phase: validate (build → test → lint), update plan status, compact context if needed
- If a phase fails validation, fix within that phase before proceeding

### 4. Intermediate Validation
- After each phase, verify the codebase is in a correct intermediate state
- Run targeted tests for affected modules, not full suite (unless risk warrants it)
- Document what changed and what remains in the plan file

### 5. Context Management for Long Tasks
- When context is growing large, create a progress summary in session memory: what's done, what's next, key decisions
- Read the progress summary at the start of each phase to re-ground context
- Compact investigation findings into structured notes rather than keeping full exploration history
- Use `manage_todo_list` for visible progress tracking throughout

### 6. Completion
- Final validation: full build + full test suite + lint
- Structured completion report with: phases completed, files changed per phase, validation evidence, any remaining risks
- Clean up intermediate session memory files

The auditor must verify that generated orchestrator agents include this pattern for complex tasks.

## Context Persistence Strategy

Generated agents must include guidance for preserving context across long or complex tasks to prevent context loss.

### When to Persist Context
- **Investigation findings**: When investigation reveals >5 files or >3 modules affected, save structured findings to session memory before starting implementation
- **Multi-step plans**: Any task with >3 discrete implementation steps should have a persisted plan
- **Long-running tasks**: When a task involves >20 file changes or crosses module boundaries, create a progress checkpoint after each major phase
- **Key decisions**: When a non-obvious architectural or implementation decision is made, record the decision and rationale in session memory

### How to Persist
- Use session memory files (`/memories/session/`) for task-specific notes — these persist within the conversation
- Use `manage_todo_list` for visible progress tracking of multi-step work
- Format: structured markdown with clear headings, file references, and status indicators
- Keep files focused: one file per concern (investigation-notes.md, migration-plan.md, progress-log.md)

### Mandatory Clarification Tool
- **Always use `vscode_askQuestions`** for structured clarification questions — never write questions as plain response text
- `vscode_askQuestions` keeps the session alive; plain-text questions end the response and break the workflow
- Provide 2-4 options with one recommended default and a free-input option
- For low-risk ambiguity, continue provisionally without asking

### Context Compaction
- When conversation context grows large, summarize completed phases into a compact progress note in session memory
- Subsequent work reads the progress note instead of relying on full conversation history
- Investigation context: keep structured findings (file list, change table), discard exploration narrative

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
- **When to promote patterns**: If you find yourself correcting the same agent behavior repeatedly, extract the correction into an instruction file with a narrow `applyTo` scope. This is the **steering loop** — improve the harness (feedforward controls), not just individual outputs.
- **When to add instructions**: When a new convention emerges that applies to specific file patterns (e.g., new test naming convention, new module boundary rule).
- **When to promote to mechanical enforcement**: When an instruction is violated repeatedly despite being documented, consider promoting the rule to a computational control (linter rule, structural test, pre-commit hook) that catches violations automatically.
- **Drift signals**: Agents referencing removed files, using outdated API patterns, or suggesting conventions the team abandoned are signs of drift — update the affected agents.
- **Entropy management**: Periodically review agent outputs for pattern degradation: growing duplication, inconsistent naming, architectural boundary violations. Address these by improving feedforward controls, not just fixing individual instances.

The generator must include this guidance section in every generated project context instruction.

## Constitution Pattern

Generated bundles must include a `<prefix>-constitution.md` instruction file (`applyTo: "**"`) that defines immutable governance rules for the project. The constitution is the highest-authority document in the generated bundle — it overrides any agent-specific instruction when they conflict.

### Structure
1. **Preamble** — Bundle identity and purpose statement
2. **Article I: Scope & Boundaries** — What the bundle covers and what it does not
3. **Article II: Evidence Standard** — Every business-rule claim must be backed by code/doc evidence, user confirmation, or labeled `[ASSUMPTION]` / `[NEEDS CLARIFICATION]`. No unanchored assertions.
4. **Article III: Review Pipeline** — Multi-stage review process requirements, short-circuit rules, verdict authority
5. **Article IV: Change Governance** — Blast-radius assessment rules, mandatory checkpoints for high-impact changes
6. **Article V: Specification Discipline** — Spec-driven pipeline requirements for non-trivial features
7. **Article VI: Context Persistence** — Session memory usage rules, progress tracking requirements
8. **Article VII: Escalation** — When and how to escalate to human decision-making
9. **Phase -1 Gates** — Constitutional gates that MUST pass before implementation begins:
   - **Simplicity Gate**: Is this the simplest approach that satisfies the requirement?
   - **Duplication Gate**: Does this duplicate existing logic? Can existing code be reused?
   - **Business Logic Gate**: Are business rules verified against source (code, docs, domain expert)?
   - **Impact Gate**: Is the blast radius understood and acceptable?

### Generation Rules
- The generator produces the constitution from project analysis — not a generic template
- Constitution articles reference actual project conventions, not placeholder guidance
- The constitution is an instruction file, not an agent — it has no workflow, just rules
- All generated agents must include a note acknowledging the constitution
- The auditor must verify constitution presence and that agents reference it

## Spec-Driven Pipeline

Generated bundles must include a spec-driven pipeline that normalizes feature work through structured specification before implementation begins.

### Pipeline Stages

1. **Refine User Input** — Normalize raw user requests into structured goal/anchor/constraints format. Validate completeness, identify ambiguity, and extract acceptance criteria. This prevents routing errors and wasted implementation cycles.
2. **Specify Feature** — Produce a feature specification (PRD) stored in `specs/<feature-id>/spec.md`. Sections: Problem Statement, Scope and Non-Goals, Acceptance Criteria (testable), Domain Rules, Edge Cases, Dependencies, Technical Constraints. The spec is the contract between planning and implementation.
3. **Plan Implementation** — Given a spec, produce an implementation plan stored in `specs/<feature-id>/plan.md`. Sections: Architecture Approach, File Changes (with dependency order), Risk Assessment, Test Strategy, Rollback Plan. The plan is reviewed before implementation begins.
4. **Generate Tasks** — Given a plan, produce ordered task breakdowns stored in `specs/<feature-id>/tasks.md`. Each task: description, files, validation criteria, estimated complexity, dependency on prior tasks. Tasks feed directly into `manage_todo_list` for execution tracking.

### Spec Workspace
- Feature specs live in `specs/<feature-id>/` directory
- Each feature gets: `spec.md`, `plan.md`, `tasks.md`
- Specs are living documents — updated as implementation reveals new constraints
- Closed specs can be archived or deleted after merge

### When to Activate
- **Always**: for features that touch >3 files, cross module boundaries, change business logic, or affect shared/core code
- **Skip**: for trivial changes (typos, small config edits, single-file fixes) where direct implementation is faster
- The orchestrator decides activation based on the task's complexity assessment

### Generated Assets
- **Refine-User-Input skill**: intake normalizer that structures raw requests
- **Specify-Feature skill**: PRD generation workflow with templates
- **Plan-Implementation skill**: architecture planning with risk assessment
- **Generate-Tasks skill**: task breakdown for execution tracking
- Templates for spec, plan, and tasks (see `spec-template.md`, `plan-template.md`, `tasks-template.md`)

## Separated Review Pipeline

Generated bundles must include a multi-stage review pipeline replacing the single Code Reviewer.

### Pipeline Structure

| Stage | Agent | Focus | Short-Circuit |
|-------|-------|-------|---------------|
| 0 | Code Review Orchestrator | Context gathering, route to reviewers | — |
| 1 | Functional Reviewer | Business correctness, AC traceability | BLOCKER → REJECT immediately |
| 2 | Technical Reviewer | Architecture, performance, security | — |
| 3 | Platform Reviewer (conditional) | Apple-specific: memory, concurrency, SwiftUI | Only when Swift/SwiftUI/UIKit files changed |

### Short-Circuit Rule
The Functional Reviewer runs first. If any **BLOCKER** severity finding is raised (broken business logic, AC violation, data integrity issue), the review pipeline **REJECT**s immediately — Technical and Platform reviews do not run. This prevents wasting review cycles on code that is fundamentally wrong from a business perspective.

### Evidence Standard in Reviews
Every review finding must include:
- **Code anchor**: file path and line reference to the exact issue
- **Evidence type**: code-backed, spec-backed, user-confirmed, `[ASSUMPTION]`, or `[NEEDS CLARIFICATION]`
- **Severity**: blocker / major / minor / suggestion
- **Fix suggestion**: concrete code-level recommendation

### When to Generate Separated Review
- Always generate the full 4-agent pipeline (Code Review Orchestrator + Functional Reviewer + Technical Reviewer + Platform Reviewer)
- Platform Reviewer is conditionally triggered — only runs when changed files include Swift/SwiftUI/UIKit code

## Handoffs Pattern

Generated agents must declare explicit delegation targets in YAML frontmatter using the `handoffs` field. This replaces implicit delegation through prose instructions and enables Copilot to offer delegation buttons in the UI.

### Frontmatter Format
```yaml
handoffs:
  - agent: "Test Specialist"
    label: "Generate Tests"
    prompt: "Generate unit tests for the implementation I just completed"
  - agent: "Code Review Orchestrator"
    label: "Review Changes"
    prompt: "Review the changes I made for business correctness and technical quality"
```

### Rules
- Every agent that delegates to other agents MUST declare handoffs
- Orchestrators have handoffs to all agents they can route to
- Specialist agents have handoffs back to the orchestrator (for escalation/completion)
- Handoff prompts should be specific enough to provide context for the receiving agent
- `label` is the user-facing button text — keep it short and action-oriented
- `agent` must be the exact display name (same as used in `agents` frontmatter)
- Handoffs supplement `agents` frontmatter — both should be present

## Evidence Standard

All generated agents must enforce a universal evidence standard for claims, findings, and recommendations:

| Label | Meaning | When to use |
|-------|---------|-------------|
| (no label) | Code/doc-backed | Default — claim verified against actual code or documentation |
| `[USER-CONFIRMED]` | Human-verified | User explicitly confirmed this fact via `vscode_askQuestions` |
| `[ASSUMPTION]` | Unverified belief | Claim based on reasonable inference but not confirmed by code, docs, or user |
| `[NEEDS CLARIFICATION]` | Ambiguous/unknown | Cannot determine truth — must ask user or investigate further before proceeding |

### Enforcement Rules
- Investigators mark uncertain findings as `[ASSUMPTION]` — implementors must verify before coding
- Business Analysts mark unconfirmed domain rules as `[NEEDS CLARIFICATION]`
- Code Reviewers must challenge any `[ASSUMPTION]` that reaches review stage without verification
- Orchestrators track assumption resolution — escalate unresolved assumptions before proceeding
- The constitution Article II codifies this standard for the project

## Runtime Docs Generation

Generated bundles must include essential documentation files that help users and maintainers operate the agent ecosystem effectively.

### User Playbook (`<prefix>-user-playbook.md`)
- How to invoke agents and use prompts
- Common workflows with example prompts
- When to use which agent
- Troubleshooting: what to do when agents produce wrong output

### Review Playbook (`<prefix>-review-playbook.md`)
- How the review pipeline works (stages, short-circuit rules)
- What each reviewer checks
- How to interpret review verdicts and severity levels
- Blast-radius review: when to request additional review for high-impact changes
- How to override or escalate review findings

### Generation Rules
- Playbooks reference actual generated agent names, not generic placeholders
- Content is derived from the actual bundle — not a static template
- User playbook is the primary onboarding doc for new team members
- Review playbook explains the separated review pipeline behavior

## Drift Detection Guidance

Generated bundles should include drift detection awareness so teams can identify when their agent ecosystem becomes stale.

### Five Drift Dimensions
1. **File references**: Agents reference files/modules that have been renamed, moved, or deleted
2. **Convention drift**: Agents enforce conventions the team has abandoned or changed
3. **API pattern drift**: Agents suggest patterns using deprecated or removed APIs
4. **Architecture drift**: Agents assume architecture that has been restructured
5. **Ecosystem drift**: Agent-to-agent references are broken (renamed/removed agents)

### Detection Triggers
- After major refactors or module restructuring
- After dependency version upgrades (especially Swift, SwiftUI, Xcode)
- After team convention changes
- Quarterly review cadence (include in bundle evolution guidance)

### Guidance Placement
- Include drift awareness in the generated project context instruction's Bundle Evolution section
- Generated orchestrators should flag suspected drift when they encounter stale references
- The auditor checks for drift signals during ecosystem audit

## Review Memory Promotion

Generated bundles must include a learning loop that promotes recurring review findings into durable prevention rules.

### Promotion Flow
1. **Detect pattern**: Code Reviewer or Functional Reviewer flags the same issue type ≥3 times
2. **Propose rule**: Draft an instruction-level rule or checklist item to prevent recurrence
3. **Validate**: Check if the rule is actionable, specific, and not already covered
4. **Promote**: Add to the relevant instruction file (path-scoped or global)
5. **Verify**: Confirm subsequent reviews no longer find the same pattern

### Where to Promote
- Convention violations → path-scoped instruction with `applyTo` for affected file patterns
- Repeated test gaps → test specialist's testing instruction
- Architecture boundary violations → project context instruction
- If violations persist after instruction promotion → escalate to mechanical enforcement (linter rule, hook)

### Generation Rules
- Generated orchestrators include a note to track review patterns
- Generated project context instruction mentions promotion as part of bundle evolution

## Hooks Generation

When the analyzer detects linting or formatting tools in the project, the generator should produce hook guidance or actual hook configurations.

### Supported Hook Patterns

| Hook | Trigger | Tool | When |
|------|---------|------|------|
| Auto-format | `postToolUse` (file edit) | SwiftFormat or swift-format | When SwiftFormat config (`.swiftformat`, `Package.swift` dependency) is detected |
| Lint check | `postToolUse` (file edit) | SwiftLint | When `.swiftlint.yml` is present |
| Compile check | `postToolUse` (file edit) | `xcodebuild build` | When project is Xcode-based and compile-on-save would catch errors early |

### Rules
- Only generate hooks when the corresponding tool is already configured in the project
- Hook files go in `.github/hooks/` or are documented in `copilot-instructions.md`
- Always include a comment explaining what the hook does and when it fires
- Compile-check hooks must use the analyzer-detected scheme and destination
- Hooks are deterministic enforcement — they must never produce false positives
- Generate hooks when the corresponding tools are detected; document the recommendation when not generating

## Generated File Marking

Every file generated by the kit must include a marker comment immediately after the YAML frontmatter closing `---` to identify it as kit-generated and support maintenance:

```
---
name: example-agent
description: "..."
---
<!-- Generated by Apple Agent Builder Kit -->
```

### Rules
- The marker is placed on the first line after the YAML frontmatter closing `---` — never before the frontmatter opening `---`
- Placing the marker before frontmatter breaks YAML parsing and causes editor diagnostics (e.g., "Skill must provide a name")
- The marker enables users, tools, and the kit itself to distinguish kit-generated files from manually authored ones
- When refreshing or updating generated files, the marker is preserved as-is (no date or prefix to update)
- The kit recognizes `<!-- Generated by Apple Agent Builder Kit -->` as the canonical marker pattern for maintenance operations
- The auditor must verify marker presence and correct placement (after frontmatter) in all generated files
- Template files within the kit itself (`.github/templates/`) do not get markers — only files generated for target projects

## Cross-Session Persistence

For tasks that may exceed a single session's context window (full app rewrites, major version migrations, large-scale refactors), generated agents must include cross-session persistence guidance:

### When to Use Cross-Session Persistence
- Task scope exceeds ~50 file changes or spans >5 modules
- Migration work that requires multiple phases over days/weeks
- Architecture-level changes that cannot be validated incrementally in one session

### How to Persist Across Sessions
- Use repository memory (`/memories/repo/`) for durable task state that survives across sessions — migration progress, phase status, key decisions, remaining work
- Use persistent files in the project (e.g., `specs/<task-id>/progress.md`) for structured migration plans and phase tracking
- Session memory (`/memories/session/`) is for within-session notes only — it does not survive across sessions
- At the end of each session, write a structured progress summary to repo memory or project files with: completed phases, current phase status, remaining phases, key decisions, blockers
- At the start of each new session, read the progress summary to re-ground context before continuing

### Plan Format for Cross-Session Work
```
# Migration Progress: {task-id}
## Status: {phase N of M}
## Completed Phases
- Phase 1: {description} — {date} — {evidence}
## Current Phase
- Phase N: {description} — {status} — {blockers}
## Remaining Phases
- Phase N+1: {description}
## Key Decisions
- {decision}: {rationale}
## Blockers
- {blocker}: {status}
```

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
