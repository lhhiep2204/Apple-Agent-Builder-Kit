# Apple Agent Builder Kit

Apple Agent Builder Kit is an Apple-first Copilot customization framework that helps teams create high-quality custom agents, skills, instructions, prompts, and templates for real software delivery work.

Instead of producing generic customization files, this kit refreshes official Copilot documentation, analyzes project context, generates focused artifacts, audits the result, and iterates until the bundle is as strong and current as the available evidence supports.

## Why This Exists

Most agent kits fail for three reasons:
- weak understanding of codebase constraints
- weak primitive selection (too many or wrong artifact types)
- weak quality control before shipping

This kit solves those issues with a role-split architecture, template-grounded generation, and hard-gated auditing.

## What This Kit Can Do

Three user-facing workflows:

- **Generate or improve full workflow** (`/generate-workflow-agents`): For any project — analyze the codebase, classify project state (greenfield, established, or mixed), then generate a full agent bundle from scratch, or audit, improve, and extend an existing ecosystem when agents already exist.
- **Add or verify a specific agent** (`/add-agent`): Create a new agent (scan existing to detect overlap, audit for ecosystem coherence), or verify and improve an existing agent (audit against kit standards, fix findings).

Both flows share the same loop: refresh docs → validate official source links → analyze → assess → ask only if needed → generate → audit → revise until `PASS`.

By default, generated artifacts use a project-derived file prefix so they are easy to distinguish from the kit's internal files.

For full workflow generation, the default expectation is a real workflow kit: a project-tailored `copilot-instructions.md`, agents, multiple focused skills, multiple narrow instructions, prompts, reusable templates, and an explicit hook decision. The generated kit should behave like a cooperating specialist team with explicit collaboration lanes, clear hand-off artifacts, and automatic iteration loops.

## Generated Workflow Coverage

When using `/generate-workflow-agents`, the kit produces agents covering the full development lifecycle:

| Role | Responsibility |
|------|---------------|
| Business Analyst | Transform feature requests into user stories with acceptance criteria and domain rules |
| Investigator | Structured as-is/to-be analysis with file references, impact matrix, scenario mapping |
| Implementor | Production code with pre-impl checklist, code reasoning, incremental implementation, verify-fix loop |
| Test Specialist | Unit tests (Swift Testing) + UI tests (XCTest), protocol-driven mocks, full coverage |
| Technical Reviewer | Deep context review (dependency graph, not just diffs), severity-driven verdicts |
| Functional Reviewer | Acceptance criteria, domain rules, edge cases; short-circuit on blockers |
| Dev Orchestrator | Auto-routes user intent to sub-agents, confirmation checkpoint, completion reports |

Default collaboration: `Business Analyst → Investigator → Implementor → Test Specialist → Reviewers`
Default iteration: `Implementor → Test Specialist → Reviewers → back to Implementor until review passes`

## Supported Platforms

iOS / iPadOS, macOS, watchOS, tvOS, visionOS

## Architecture

Conductor + specialist subagent pattern. Kit subagents use the "Apple" prefix — generated dev workflow agents do not.

```
Agent Builder (Conductor)
├── Apple Copilot Docs Refresher — refreshes official Copilot docs and builds the refresh brief
├── Apple Codebase Analyzer   — reads project, inventories existing agents
├── Apple Agent Generator      — generates artifacts using scaffold templates
└── Apple Quality Auditor      — hard-gates the bundle before finalization
```

Templates in `.github/templates/agent-builder/` are wired directly into each subagent:
- Analyzer uses `apple-codebase-analysis-template.md` for structured output
- Generator uses scaffold templates (`agent/skill/instruction/prompt-template.md`, `workflow-asset-template.md`), `apple-role-catalog.md`, `primitive-decision-matrix.md`, `hook-checklist.md`
- Documentation refresh uses `copilot-doc-source-registry.md` (authoritative source registry) and `kit-doc-refresh.md` (persistent refresh snapshot)
- Auditor uses `agent-audit-rubric.md` (6 dimensions: discovery, architecture fit, execution quality, Apple specificity, scope discipline, ecosystem coherence)

## Repo Structure

```text
.github/
  copilot-instructions.md              ← repository-wide guidance
  agents/
    agent-builder.agent.md              ← conductor
    apple-copilot-docs-refresher.agent.md ← subagent: refreshes official Copilot docs
    apple-codebase-analyzer.agent.md    ← subagent: reads project + existing agents
    apple-agent-generator.agent.md      ← subagent: generates bundle from templates
    apple-quality-auditor.agent.md      ← subagent: hard-gates before finalize
  skills/
    agent-builder/SKILL.md              ← SSoT for workflow, bundle shapes, artifact requirements
  instructions/
    agent-builder.instructions.md
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
      copilot-doc-refresh-brief-template.md ← refresh brief format
      copilot-doc-source-registry.md    ← URL registry for official Copilot docs
      kit-doc-refresh.md                ← last documentation refresh (kit-internal)
      agent-audit-rubric.md             ← audit checklist (6 dimensions)
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
4. The kit analyzes your codebase, refreshes official Copilot docs, generates agents grounded in your conventions, and audits everything automatically.
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
- `myapp-technical-reviewer.agent.md` — concurrency, architecture, performance
- `myapp-functional-reviewer.agent.md` — business rules, edge cases, offline behavior
- Updated `copilot-instructions.md` — broad project context shared by all agents
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
- **Simulator destination**: Derive from project's deployment target + Xcode version, never hardcode device model
- **Lint validation**: When linter is configured, verify with `--strict` flag to catch warnings, not just errors
- Comprehensive artifact set by default — include every artifact that materially improves execution quality
- Official-doc refresh before generation — fetch current sources, repair broken links
- Linting tools best handled via instructions and agent validation steps; hooks only when justified
- Project-derived file prefixes for generated artifacts
