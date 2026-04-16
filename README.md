# Apple Agent Builder Kit

Apple Agent Builder Kit is an Apple-first Copilot customization framework that helps teams create high-quality custom agents, skills, instructions, prompts, and templates for real software delivery work.

Instead of producing generic customization files, this kit analyzes project context, generates focused artifacts, audits the result, and iterates until the bundle is as strong and current as the available evidence supports. Official Copilot documentation is maintained as a persistent kit reference file, while Apple platform domain knowledge is derived from the target project's own code, config, tests, resources, and capabilities. When MCP GitHub is available, the kit also discovers relevant community skills from the [Swift Agent Skills](https://github.com/twostraws/Swift-Agent-Skills) directory to enrich generated agents with community-curated domain knowledge.

## Why This Exists

Most agent kits fail for three reasons:
- weak understanding of codebase constraints
- weak primitive selection (too many or wrong artifact types)
- weak quality control before shipping

This kit solves those issues with a role-split architecture, template-grounded generation, and hard-gated auditing.

## What This Kit Can Do

Three user-facing entry points:

- **Generate or improve full workflow** (`/generate-workflow-agents`): For any project — analyze the codebase, classify project state (greenfield, established, or mixed), then generate a full agent bundle from scratch, or audit, improve, and extend an existing ecosystem when agents already exist.
- **Add or verify a specific agent** (`/add-agent`): Create a new agent (scan existing to detect overlap, audit for ecosystem coherence), or verify and improve an existing agent (audit against kit standards, fix findings).
- **Refresh existing agents** (`/refresh-agents`): Re-analyze an evolved codebase, detect drift across five dimensions, and surgically update only the affected agents, instructions, and skills without full regeneration.

All flows share the same loop: analyze → assess → ask only if needed → generate → audit → revise until `PASS`. The entire workflow completes in a single session — when clarification is needed, the kit uses `vscode_askQuestions` to present structured options, waits for your response, and continues. It never abandons generation mid-session.

By default, generated artifacts use a project-derived file prefix so they are easy to distinguish from the kit's internal files. The intentional exception is `.github/copilot-instructions.md`, which keeps the standard Copilot filename and is created or updated in the target project.

For full workflow generation, the default expectation is a real workflow kit: an updated or newly created `.github/copilot-instructions.md` (workspace-level overview and agent guide), a constitution, a project-tailored project context instruction (`<prefix>-project-context.instructions.md`), agents with a separated review pipeline, multiple focused skills including a spec-driven pipeline, multiple narrow instructions, prompts, reusable templates, runtime docs, and an explicit hook decision. The generated kit should behave like a cooperating specialist team with explicit collaboration lanes, clear hand-off artifacts via `handoffs:` frontmatter, and automatic iteration loops.

For business-heavy projects, the kit can also generate shared business knowledge artifacts when they earn their place: domain-scoped instructions, a business domain registry / domain map, or a reusable business-domain skill. Small or simple projects should keep business context in the project context instruction instead of forcing extra files.

## Generated Workflow Coverage

When using `/generate-workflow-agents`, the kit produces agents covering the full development lifecycle:

| Role | Responsibility |
|------|---------------|
| Dev Orchestrator | Auto-routes user intent to the right specialist, coordinates the full delivery pipeline, manages iteration loops, single-session completion, non-blocking clarification via `vscode_askQuestions` |
| Business Analyst | Transform feature requests into user stories with acceptance criteria and domain rules |
| Investigator | Structured as-is/to-be analysis producing repository impact maps with file references, real symbol names, and dependency-ordered change groups |
| Implementor | Production code with Phase -1 gates, pre-impl checklist, code reasoning, incremental implementation, verify-fix loop (build → test → lint --strict) |
| Test Specialist | Unit tests (Swift Testing) + UI tests (XCTest), protocol-driven mocks, full coverage |
| Code Review Orchestrator | Orchestrates multi-stage review pipeline, aggregates verdicts, enforces short-circuit on blockers |
| Functional Reviewer | Business correctness, AC ↔ Test ↔ Code traceability, edge cases. BLOCKER → REJECT immediately (short-circuit) |
| Technical Reviewer | Architecture, performance, security, layer responsibility. Runs only if functional review passes |
| Platform Reviewer | Apple-specific: memory management, concurrency, SwiftUI recomposition, accessibility. Conditional — only when Swift/SwiftUI files changed |

Default collaboration: `Business Analyst → Investigator → [confirmation checkpoint] → Implementor → Test Specialist → Code Review Orchestrator`
Default iteration: `Implementor → Test Specialist → Code Review Orchestrator → back to Implementor until review passes (max 3 cycles)`

### Separated Review Pipeline

The review pipeline is split into specialized stages with a short-circuit rule:

```
Stage 0: Context Gathering (Code Review Orchestrator)
    → Read full file content + dependency graph + callers
Stage 1: Functional Review → BLOCKER? → REJECT immediately
Stage 2: Technical Review (only if Stage 1 passes)
Stage 3: Platform Review (only when Swift/SwiftUI files changed)
    → Combined verdict: PASS / REVISE / REJECT
```

### Spec-Driven Pipeline

For non-trivial features (>3 files, cross-module, business logic changes), the kit activates a structured specification pipeline before implementation:

1. **Refine User Input** — Normalize raw requests into goal/anchor/constraints format
2. **Specify Feature** — Produce feature specification (`specs/<id>/spec.md`)
3. **Plan Implementation** — Architecture approach, file changes, risk assessment (`specs/<id>/plan.md`)
4. **Generate Tasks** — Ordered task breakdown for execution tracking (`specs/<id>/tasks.md`)

### Constitution

Every generated bundle includes a `<prefix>-constitution.md` instruction file — the highest-authority document in the bundle. It defines immutable governance rules: scope boundaries, evidence standard, review pipeline rules, change governance, specification discipline, context persistence, escalation rules, and Phase -1 gates (Simplicity, Duplication, Business Logic, Impact) that must pass before implementation begins.

### Evidence Standard

All generated agents enforce a universal evidence standard: every claim must be backed by code/doc evidence, user confirmation, or labeled `[ASSUMPTION]` / `[NEEDS CLARIFICATION]`. Investigators mark uncertain findings, implementors verify before coding, reviewers challenge unresolved assumptions, and orchestrators track resolution across phases.

### Community Skill Discovery (MCP GitHub Conditional)

When MCP GitHub tools are available during generation, the analyzer automatically discovers relevant community skills from the [Swift Agent Skills](https://github.com/twostraws/Swift-Agent-Skills) directory — a curated collection of open-source AI agent skills for Swift and Apple platform development.

**How it works**:
1. After determining the project's tech stack (Technology Alignment Profile), the analyzer reads the Swift Agent Skills README via MCP GitHub
2. Skill categories are matched against the project's detected technologies (SwiftUI, SwiftData, Swift Concurrency, Swift Testing, accessibility, security, etc.)
3. For matching categories, the analyzer follows links into individual skill repositories to extract actionable domain knowledge
4. The generator embeds core knowledge into relevant generated agents and recommends community skills for user installation in the user playbook

**Precedence rule**: Project-specific code patterns always override community skill guidance when they conflict.

**Graceful degradation**: When MCP GitHub is not available, the kit operates normally using only project-specific evidence and kit knowledge — no core functionality is lost.

## Supported Platforms

iOS / iPadOS, macOS, watchOS, tvOS, visionOS

## Architecture

Conductor + specialist subagent pattern. Kit subagents use the "Apple" prefix — generated dev workflow agents do not.

The kit intentionally keeps one internal source-of-truth skill plus specialized subagents. That is by design: the kit's own specialization lives in the subagents, while generated target-project bundles may contain multiple project-specific skills, domain-scoped instructions, and optional business-domain artifacts when the target codebase warrants them.

```
Agent Builder (Conductor)
├── Apple Codebase Analyzer     — reads project, inventories existing agents
├── Apple Agent Generator       — generates artifacts using scaffold templates
├── Apple Quality Auditor       — hard-gates the bundle before finalization
│
│  Kit maintenance only (not invoked during generation):
└── Apple Copilot Docs Refresher — refreshes official Copilot docs into kit-doc-refresh.md
```

Templates in `.github/templates/agent-builder/` are wired directly into each subagent:
- Analyzer uses `apple-codebase-analysis-template.md` for structured output, including the Technology Alignment Profile and Apple Domain Coverage Matrix built from target-project evidence
- Analyzer discovers community skills from `twostraws/Swift-Agent-Skills` via MCP GitHub when available, matching categories to the project's tech stack and extracting domain knowledge for the generator
- Generator uses scaffold templates (`agent/skill/instruction/prompt-template.md`, `workflow-asset-template.md`), `apple-role-catalog.md`, `primitive-decision-matrix.md`, `hook-checklist.md`, plus reads `kit-doc-refresh.md` (Copilot product behavior), the analyzer's Apple domain evidence, and community skill discovery results as its inputs
- Documentation refresh (kit maintenance only) uses `kit-doc-source-registry.md` (authoritative source registry) and updates `kit-doc-refresh.md`
- Auditor uses `agent-audit-rubric.md` (7 dimensions: discovery, architecture fit, execution quality, Apple specificity, scope discipline, ecosystem coherence, context efficiency)
- Business knowledge persistence is decided by the analyzer + generator together: keep simple projects in the project context instruction, and add domain-scoped instructions, a business domain registry / domain map, or a business-domain skill only when the target project's domain complexity warrants it

## Repo Structure

```text
.github/

  agents/
    agent-builder.agent.md              ← conductor
    apple-copilot-docs-refresher.agent.md ← kit maintenance: refreshes official Copilot docs
    apple-codebase-analyzer.agent.md    ← subagent: reads project + existing agents
    apple-agent-generator.agent.md      ← subagent: generates bundle from templates
    apple-quality-auditor.agent.md      ← subagent: hard-gates before finalize
  skills/
    agent-builder/SKILL.md              ← SSoT for workflow, bundle shapes, artifact requirements
  instructions/
    agent-builder.instructions.md       ← kit-wide guidance + customization file standards
  prompts/
    generate-workflow-agents.prompt.md  ← generates, verifies, or improves full dev workflow kit
    add-agent.prompt.md                 ← adds a new agent or verifies an existing agent
    refresh-agents.prompt.md            ← re-analyzes codebase, detects drift, surgically updates agents
  templates/
    agent-builder/
      agent-template.md                 ← scaffold: agent
      skill-template.md                 ← scaffold: skill
      instruction-template.md           ← scaffold: instruction
      prompt-template.md                ← scaffold: prompt
      workflow-asset-template.md        ← scaffold: templates/checklists (includes common patterns)
      constitution-template.md          ← scaffold: constitution instruction
      apple-codebase-analysis-template.md ← analyzer output format
      kit-doc-refresh-template.md        ← refresh brief output format (used by refresher)
      kit-doc-source-registry.md        ← URL registry for official Copilot docs (used by refresher)
      kit-doc-refresh.md                ← persistent Copilot product behavior reference (read by generator)
      agent-audit-rubric.md             ← audit checklist (7 dimensions)
      primitive-decision-matrix.md      ← primitive selection decision aid
      apple-role-catalog.md             ← role naming reference
      hook-checklist.md                 ← hook decision checklist
      spec-template.md                  ← scaffold: feature specification
      plan-template.md                  ← scaffold: implementation plan
      tasks-template.md                 ← scaffold: task breakdown
      review-playbook-template.md       ← scaffold: review playbook
      user-playbook-template.md         ← scaffold: user playbook
```

## Quick Start

1. Copy `.github/` into your Apple platform repository.
2. Open Copilot Chat.
3. Use the prompt entry points:
  - `/generate-workflow-agents` — build, verify, or refresh the full dev workflow kit
  - `/add-agent` — add one new agent or verify an existing agent without breaking ecosystem coherence
  - `/refresh-agents` — re-analyze an evolved codebase and surgically update drifted agents
4. The kit analyzes your codebase, generates agents grounded in your conventions, and audits everything automatically.
5. Generated files use a project-derived prefix so they stay visually separate from the kit's own assets.
6. Every generated file includes a `<!-- Generated by Apple Agent Builder Kit -->` marker after the YAML frontmatter for maintenance traceability.

## Real Examples

### Example A: Generate a Full Development Workflow Kit

```
/generate-workflow-agents
```

The kit reads the project, detects project state, then generates a full bundle including:

**Agents** (9 agents):
- `myapp-dev-orchestrator.agent.md` — sequences all stages, auto-routes intent
- `myapp-business-analyst.agent.md` — user stories and acceptance criteria
- `myapp-investigator.agent.md` — repository impact maps with file/symbol references
- `myapp-implementor.agent.md` — production code with verify-fix loop
- `myapp-test-specialist.agent.md` — unit and UI tests with full coverage
- `myapp-code-review-orchestrator.agent.md` — multi-stage review pipeline
- `myapp-functional-reviewer.agent.md` — business correctness review with short-circuit
- `myapp-technical-reviewer.agent.md` — architecture, performance, security review
- `myapp-platform-reviewer.agent.md` — Apple-specific: memory, concurrency, SwiftUI

**Governance**:
- `myapp-constitution.instructions.md` — immutable governance rules, Phase -1 gates, evidence standard

**Skills** (5+ skills):
- `myapp-delivery-workflow/SKILL.md` — end-to-end delivery pipeline
- `myapp-spec-pipeline/SKILL.md` — spec-driven feature pipeline
- `myapp-investigation/SKILL.md` — investigation workflow
- `myapp-review/SKILL.md` — review pipeline protocol
- `myapp-testing/SKILL.md` — testing methodology

**Instructions** (3+ instructions):
- Created or updated `.github/copilot-instructions.md` — concise workspace-level overview and agent ecosystem guide
- `myapp-project-context.instructions.md` — deeper project context shared by all agents
- `myapp-implementation.instructions.md` — implementation conventions
- `myapp-test.instructions.md` — test-specific conventions
- Additional path-scoped instructions when project signal warrants them (e.g., SwiftUI conventions scoped to Presentation layer)

**Prompts** (3+ prompts):
- `myapp.prompt.md` — primary entry point (routes through Dev Orchestrator)
- `myapp-spec.prompt.md` — spec-driven feature pipeline entry
- `myapp-review.prompt.md` — code review entry point

**Templates** (3+ templates):
- `myapp/spec-template.md` — feature specification template
- `myapp/plan-template.md` — implementation plan template
- `myapp/tasks-template.md` — task breakdown template

**Docs** (2 docs):
- `myapp-user-playbook.md` — how to use the agent ecosystem
- `myapp-review-playbook.md` — how the review pipeline works

**Other**:
- Hook guidance or explicit no-hook rationale

### Example B: Verify and Improve Existing Agents

```
/generate-workflow-agents
```

On a project with existing agents, the kit audits the current ecosystem, identifies weak roles, missing hand-offs, and overlap, then proposes and applies improvements until the bundle passes audit.

### Example C: Add a Specialist Agent

```
/add-agent
Add a performance profiling agent that helps identify memory leaks,
excessive CPU usage, and slow SwiftUI rendering. It should analyze
Instruments traces and suggest targeted fixes.
```

The kit scans existing agents to detect scope and naming, then generates an integrated specialist with no overlap, audited for ecosystem coherence.

### Example D: Refresh After Codebase Evolution

```
/refresh-agents
```

The kit re-analyzes the project, detects drift across five dimensions (file references, conventions, API patterns, architecture, ecosystem), presents a structured drift report, and surgically updates only the affected files while preserving manual customizations.

## Three Prompt Entry Points

| Prompt | Use when | Flows |
|--------|----------|-------|
| `/generate-workflow-agents` | Any development project needs a workflow kit | Greenfield (A), Established (B), Mixed (B variant) |
| `/add-agent` | Need to add one agent, or verify an existing agent | Create/Extend (C), Verify single agent |
| `/refresh-agents` | Codebase evolved, agents may be stale | Drift detection → Surgical update (D) |

## Generated File Marking

Every file generated by the kit includes a marker comment immediately after the YAML frontmatter:

```markdown
---
name: Example Agent
description: "..."
---
<!-- Generated by Apple Agent Builder Kit -->
```

The marker is placed **after** the frontmatter closing `---`, never before it (placing it before frontmatter breaks YAML parsing and causes editor diagnostics). The kit recognizes this exact pattern during refresh and maintenance operations to identify generated files.

## Quality Principles

- Apple-first defaults: Swift 6, SwiftUI, `@Observable`, Swift Testing, SwiftData
- Legacy-aware fallback: UIKit, AppKit, Core Data, XCTest UI when the codebase requires it
- **Technology alignment**: Use project's actual stack (not kit defaults) when generating agents
- **Generation Principles**: Every generated agent embeds 8 non-negotiable behavioral rules (understand before changing, confirm business logic, no duplicate validation, respect boundaries, clarify before acting via `vscode_askQuestions`, verify before claiming, prefer simplicity, explain decisions)
- **Separated review pipeline**: Code Review Orchestrator + Functional Reviewer + Technical Reviewer + Platform Reviewer with short-circuit on blockers
- **Spec-driven pipeline**: Structured specification before implementation for non-trivial features
- **Constitution**: Immutable governance with Phase -1 gates and evidence standard
- **Handoffs**: Explicit delegation via `handoffs:` YAML frontmatter for UI-driven agent routing
- **Evidence standard**: Every claim backed by code/doc evidence, user confirmation, or labeled `[ASSUMPTION]` / `[NEEDS CLARIFICATION]`
- **Context optimization**: Workspace-level `copilot-instructions.md` provides the concise foundation, then the context model distributes deeper facts across project context instruction, path-scoped instructions, and agent instructions — no duplication, concise hand-offs
- **Domain knowledge placement**: Do not generate one skill per detected technology area. Create domain skills only when heavily used AND multiple agents need them. Use instructions for small convention sets, embed in agent for single-agent domains, skip for thin signal
- **Business knowledge placement**: Store business context in the lightest shared artifact that fits the project — project context instruction for simple products, domain-scoped instructions or a business domain registry / domain map for richer products, and a business-domain skill only when multiple agents reuse the same business workflow
- **Harness engineering**: Distinguish feedforward (guides) from feedback (sensors), leverage computational controls (linters, tests) and inferential controls (AI review), structured investigation outputs for agent legibility
- **Bundle evolution**: Generated project context instruction includes maintenance guidance so teams know when to update agents, promote patterns, and detect drift
- **Drift detection**: For established projects, the analyzer checks for stale file references, outdated conventions, and abandoned patterns before generation
- **Large task support**: Planning lane for complex tasks (>10 files, >3 modules), cross-session persistence for migrations (>50 files, >5 modules)
- **Simulator destination**: Derive from project's deployment target + Xcode version, never hardcode device model
- **Lint validation**: When linter is configured, verify with `--strict` flag to catch warnings, not just errors
- **Tool surface default**: Generated agents should not declare frontmatter `tools` or `mcp-servers` unless explicitly requested — preserves access to all tools including user-configured MCP servers
- **Community skill enrichment**: When MCP GitHub is available, the analyzer discovers community skills from `twostraws/Swift-Agent-Skills`, the generator embeds extracted knowledge into relevant agents, and the user playbook recommends skill installation. Project patterns always take precedence over community skill guidance
- **Single-session completion**: Entire workflow completes in one session. Uses `vscode_askQuestions` for structured clarification, never plain-text questions that end the session
- Comprehensive artifact set by default — include every artifact that materially improves execution quality
- Kit reference files maintained separately — Copilot docs snapshot is a persistent kit asset, while Apple domain knowledge comes from codebase-first analysis
- Linting tools best handled via instructions and agent validation steps; hooks only when justified
- Project-derived file prefixes for generated artifacts
