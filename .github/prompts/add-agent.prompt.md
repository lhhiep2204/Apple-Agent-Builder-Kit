---
description: "Add a new Apple-platform Copilot agent or verify an existing one: analyze current agents, then generate a well-scoped new agent or audit a specific agent for consistency and effectiveness with the agent kit."
agent: "Agent Builder"
---

# Add or Verify Agent

Classify user intent first, then execute the matching flow:

- **Create** — User describes a new agent to add → Execute Flow C (Extend)
- **Verify** — User points to an existing agent (with its skill, instruction, prompt files) and asks to verify, audit, or check consistency → Execute Verify flow

When intent is ambiguous, check whether the user referenced existing files or described a new workflow. If still unclear, ask one clarifying question.

## Standard User-Facing Progress

1. Analyze the codebase
2. Assess and choose bundle architecture (Create) or evaluate target agent (Verify)
3. Ask targeted follow-up only if needed
4. Generate or update the bundle (Create) or fix and improve the target agent (Verify)
5. Audit and iterate until PASS
6. Deliver the final result

Phase 3 auto-skips when analysis provides enough context.

Complete the entire workflow through delivery in this session. When user input is needed, use `vscode_askQuestions` to ask with structured options and wait for the response, then continue. Never write questions as plain text. Never leave generation incomplete.

---

## Create Flow

Describe the agent you want to add below. Include: workflow or role, target users, key tasks, and autonomy constraints.

### Execution Steps

1. **Analyze** — Scan existing agents, skills, instructions, prompts. Understand conventions the new agent must follow. Identify overlap/gaps. If business-domain context is important, recommend where it should live (concise project context instruction summary, domain-scoped instructions, business domain registry / domain map, or business-domain skill). When MCP GitHub tools are available, discover relevant community skills from `twostraws/Swift-Agent-Skills` matched to the project's tech stack.
2. **Assess** — Current ecosystem shape, overlap risks, integration needs, conductor routing changes if needed.
3. **Clarify only if needed** — Ask minimum targeted questions for missing critical facts.
4. **Generate** — Create new agent with: no role overlap, consistent naming, project-derived prefix, real conventions, supporting files (skill, instruction, prompt, template) when needed. Always create or update the target project's `.github/copilot-instructions.md` so the workspace-level guide stays aligned with the current agent ecosystem, while preserving relevant existing content. Create or update the paired `<prefix>-project-context.instructions.md` whenever shared project context or conventions should be reused across agents, and update it if it already exists. If business complexity warrants it, generate the lightest effective shared business artifact and wire it explicitly into relevant agent inputs/decision rules. Follow YAML frontmatter determinism rules: `agents` uses inline array with display names, `description` is double-quoted, no `tools`/`mcp-servers` unless requested. Include MCP tool preference for external service access.
5. **Audit** — All dimensions + ecosystem coherence. If `REVISE`, fix until `PASS`.
6. **Deliver** — Assessment, agent file(s), integration guide, changes needed to existing agents.

Final quality expectation: zero open findings (including minor) and zero editor diagnostics in generated or updated agent files before completion.

If the repository has unrelated changes, preserve them and continue unless there is a direct file-path conflict.

### Example

> Add a performance profiling agent that helps identify and fix memory leaks, excessive CPU usage, and slow UI rendering in SwiftUI views.

---

## Verify Flow

Point to an existing agent and its associated files (skill, instruction, prompt). The kit will verify consistency, quality, and effectiveness against agent kit standards.

### Execution Steps

1. **Collect target** — Identify the agent file and all associated files (skill, instruction, prompt, template) the user wants verified. Read every file fully.
2. **Analyze ecosystem** — Scan the rest of the project's agents and conventions to understand the context the target agent operates in.
3. **Evaluate** — Check the target agent and its associated files against the audit rubric:
   - Frontmatter validity and description discoverability
   - Role clarity, mission, non-goals, output contract
   - Scope discipline and `applyTo` narrowness
   - Cross-reference integrity between agent ↔ skill ↔ instruction ↔ prompt
   - Business knowledge persistence fit: if business artifacts exist (domain map/registry, domain-scoped instructions, business-domain skill), verify relevant agents explicitly consume them
   - Naming consistency with project conventions
   - Apple platform specificity where applicable
   - Overlap or conflict with other agents in the ecosystem
   - Behavioral pattern correctness per SKILL.md artifact requirements
4. **Fix** — Propose concrete fixes for every finding. Apply fixes with user approval. Do not skip minor findings.
5. **Audit** — Run full audit on the corrected files. If `REVISE`, fix and re-audit until `PASS`.
6. **Deliver** — Summary of findings, changes made, remaining risks, and ecosystem coherence status.

### Example

> Verify my Swift Testing agent — check if `swift-testing.agent.md`, its skill in `skills/swift-testing/SKILL.md`, and instruction `swift-testing.instructions.md` are consistent and follow kit best practices.
