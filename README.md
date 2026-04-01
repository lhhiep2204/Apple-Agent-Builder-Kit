# Apple Agent Builder Kit

Apple Agent Builder Kit is an Apple-first Copilot customization framework that helps teams create high-quality custom agents, skills, instructions, prompts, and templates for real software delivery work.

Instead of producing generic customization files, this kit analyzes project context, generates focused artifacts, audits the result, and iterates until the bundle is as strong and current as the available evidence supports. Official Copilot documentation is maintained as a persistent kit reference file, while Apple platform domain knowledge is derived from the target project's own code, config, tests, resources, and capabilities.

## Why This Exists

Most agent kits fail for three reasons:
- weak understanding of codebase constraints
- weak primitive selection (too many or wrong artifact types)
- weak quality control before shipping

This kit solves those issues with a role-split architecture, template-grounded generation, and hard-gated auditing.

## What This Kit Can Do

Two user-facing entry points, covering three workflows:

- **Generate or improve full workflow** (`/generate-workflow-agents`): For any project — analyze the codebase, classify project state (greenfield, established, or mixed), then generate a full agent bundle from scratch, or audit, improve, and extend an existing ecosystem when agents already exist.
- **Add or verify a specific agent** (`/add-agent`): Create a new agent (scan existing to detect overlap, audit for ecosystem coherence), or verify and improve an existing agent (audit against kit standards, fix findings).

Both flows share the same loop: analyze → assess → ask only if needed → generate → audit → revise until `PASS`. The entire workflow completes in a single session — when clarification is needed, the kit asks with structured options, waits for your response, and continues. It never abandons generation mid-session.

By default, generated artifacts use a project-derived file prefix so they are easy to distinguish from the kit's internal files. The intentional exception is `.github/copilot-instructions.md`, which keeps the standard Copilot filename and is created or updated in the target project.

For full workflow generation, the default expectation is a real workflow kit: an updated or newly created `.github/copilot-instructions.md` (workspace-level overview and agent guide), a project-tailored project context instruction (`<prefix>-project-context.instructions.md`), agents, multiple focused skills, multiple narrow instructions, prompts, reusable templates, and an explicit hook decision. The generated kit should behave like a cooperating specialist team with explicit collaboration lanes, clear hand-off artifacts, and automatic iteration loops.

For business-heavy projects, the kit can also generate shared business knowledge artifacts when they earn their place: domain-scoped instructions, a business domain registry / domain map, or a reusable business-domain skill. Small or simple projects should keep business context in the project context instruction instead of forcing extra files.

## Generated Workflow Coverage

When using `/generate-workflow-agents`, the kit produces agents covering the full development lifecycle:

| Role | Responsibility |
|------|---------------|
| Business Analyst | Transform feature requests into user stories with acceptance criteria and domain rules |
| Investigator | Structured as-is/to-be analysis with file references, impact matrix, scenario mapping |
| Implementor | Production code with pre-impl checklist, code reasoning, incremental implementation, verify-fix loop |
| Test Specialist | Unit tests (Swift Testing) + UI tests (XCTest), protocol-driven mocks, full coverage |
| Code Reviewer | Thorough combined technical + functional review: architecture, concurrency, performance, security, business rules, acceptance criteria, edge cases; PR review and general code change review with severity-driven verdicts |
| Dev Orchestrator | Auto-routes user intent to sub-agents, confirmation checkpoint, completion reports, single-session completion, non-blocking clarification (options + recommended default + provisional continuation) |

Default collaboration: `Business Analyst → Investigator → Implementor → Test Specialist → Code Reviewer`
Default iteration: `Implementor → Test Specialist → Code Reviewer → back to Implementor until review passes`

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
- Generator uses scaffold templates (`agent/skill/instruction/prompt-template.md`, `workflow-asset-template.md`), `apple-role-catalog.md`, `primitive-decision-matrix.md`, `hook-checklist.md`, plus reads `kit-doc-refresh.md` (Copilot product behavior) and the analyzer's Apple domain evidence as its inputs
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
  templates/
    agent-builder/
      agent-template.md                 ← scaffold: agent
      skill-template.md                 ← scaffold: skill
      instruction-template.md           ← scaffold: instruction
      prompt-template.md                ← scaffold: prompt
      workflow-asset-template.md        ← scaffold: templates/checklists (includes common patterns)
      apple-codebase-analysis-template.md ← analyzer output format
      kit-doc-refresh-template.md        ← refresh brief output format (used by refresher)
      kit-doc-source-registry.md        ← URL registry for official Copilot docs (used by refresher)
      kit-doc-refresh.md                ← persistent Copilot product behavior reference (read by generator)
      agent-audit-rubric.md             ← audit checklist (7 dimensions)
      primitive-decision-matrix.md      ← primitive selection decision aid
      apple-role-catalog.md             ← role naming reference
      hook-checklist.md                 ← hook decision checklist
```

## Quick Start

1. Copy `.github/` into your Apple platform repository.
2. Open Copilot Chat.
3. Use the prompt entry points:
  - `/generate-workflow-agents` — build, verify, or refresh the full dev workflow kit
  - `/add-agent` — add one new agent or verify an existing agent without breaking ecosystem coherence
4. The kit analyzes your codebase, generates agents grounded in your conventions, and audits everything automatically.
5. Generated files use a project-derived prefix so they stay visually separate from the kit's own assets.

## Real Examples

### Example A: Generate a Full Development Workflow Kit

```
/generate-workflow-agents
```

The kit reads the project, detects project state, then generates:
- `myapp-dev-orchestrator.agent.md` — sequences all stages end-to-end
- `myapp-business-analyst.agent.md` — user stories and acceptance criteria
- `myapp-investigator.agent.md` — feature investigation and impact analysis
- `myapp-implementor.agent.md` — production code across all architecture layers
- `myapp-test-specialist.agent.md` — unit and UI tests with full coverage
- `myapp-code-reviewer.agent.md` — technical + functional review, PR review, severity-driven verdicts
- Created or updated `.github/copilot-instructions.md` — concise workspace-level project overview, conventions, and agent ecosystem guide
- Created or updated `myapp-project-context.instructions.md` — deeper project context shared by all agents
- `myapp-delivery-workflow/SKILL.md` — reusable delivery orchestration skill
- `myapp-testing-methodology/SKILL.md` — testing patterns shared across agents
- `myapp-implementation.instructions.md` — narrow implementation conventions
- `myapp-tests.instructions.md` — test-specific conventions
- `myapp-deliver-feature.prompt.md` — primary entry point
- `myapp-investigate.prompt.md` — secondary entry point
- `myapp-feature-handoff-checklist.md` — reusable feature delivery template
- `myapp-investigation-report-template.md` — reusable investigation template
- Hook guidance or a no-hook rationale

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

### Example D: Verify an Existing Agent

```
/add-agent
Verify my Swift Testing agent — check if `swift-testing.agent.md`,
its skill in `skills/swift-testing/SKILL.md`, and instruction
`swift-testing.instructions.md` are consistent and follow kit best practices.
```

The kit reads the target agent and all associated files, compares against the audit rubric, proposes concrete fixes, and re-audits until PASS.

## Two Prompt Entry Points

| Prompt | Use when | Flows |
|--------|----------|-------|
| `/generate-workflow-agents` | Any development project needs a workflow kit | Greenfield (A), Established (B), Mixed (B variant) |
| `/add-agent` | Need to add one agent, or verify an existing agent | Create/Extend (C), Verify single agent |

## Quality Principles

- Apple-first defaults: Swift 6, SwiftUI, `@Observable`, Swift Testing, SwiftData
- Legacy-aware fallback: UIKit, AppKit, Core Data, XCTest UI when the codebase requires it
- **Technology alignment**: Use project's actual stack (not kit defaults) when generating agents
- **Generation Principles**: Every generated agent embeds 8 non-negotiable behavioral rules (understand before changing, confirm business logic, no duplicate validation, respect boundaries, clarify before acting, verify before claiming, prefer simplicity, explain decisions)
- **Context optimization**: Workspace-level `copilot-instructions.md` provides the concise foundation, then the context model distributes deeper facts across project context instruction, path-scoped instructions, and agent instructions — no duplication, concise hand-offs
- **Business knowledge placement**: Store business context in the lightest shared artifact that fits the project - project context instruction for simple products, domain-scoped instructions or a business domain registry / domain map for richer products, and a business-domain skill only when multiple agents reuse the same business workflow
- **Bundle evolution**: Generated project context instruction includes maintenance guidance so teams know when to update agents, promote patterns, and detect drift
- **Drift detection**: For established projects, the analyzer checks for stale file references, outdated conventions, and abandoned patterns before generation
- **Simulator destination**: Derive from project's deployment target + Xcode version, never hardcode device model
- **Lint validation**: When linter is configured, verify with `--strict` flag to catch warnings, not just errors
- **Tool surface default**: Generated agents should not declare frontmatter `tools` or `mcp-servers` unless explicitly requested
- Comprehensive artifact set by default — include every artifact that materially improves execution quality
- Kit reference files maintained separately — Copilot docs snapshot is a persistent kit asset, while Apple domain knowledge comes from codebase-first analysis
- Linting tools best handled via instructions and agent validation steps; hooks only when justified
- Project-derived file prefixes for generated artifacts
