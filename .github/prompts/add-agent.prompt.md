---
description: "Add a new Apple-platform Copilot agent or verify an existing one: refresh docs, analyze current agents, then generate a well-scoped new agent or audit a specific agent for consistency and effectiveness with the agent kit."
agent: "Agent Builder"
---

# Add or Verify Agent

Classify user intent first, then execute the matching flow:

- **Create** — User describes a new agent to add → Execute Flow C (Extend)
- **Verify** — User points to an existing agent (with its skill, instruction, prompt files) and asks to verify, audit, or check consistency → Execute Verify flow

When intent is ambiguous, check whether the user referenced existing files or described a new workflow. If still unclear, ask one clarifying question.

## Standard User-Facing Progress

1. Refresh Copilot documentation and Swift domain knowledge
2. Analyze the codebase
3. Assess and choose bundle architecture (Create) or evaluate target agent (Verify)
4. Ask targeted follow-up only if needed
5. Generate or update the bundle (Create) or fix and improve the target agent (Verify)
6. Audit and iterate until PASS
7. Deliver the final result

Phase 4 auto-skips when analysis provides enough context.

Complete the entire workflow — from refresh through delivery — in this session. When user input is needed, ask with structured options and wait for the response, then continue. Never leave generation incomplete.

---

## Create Flow

Describe the agent you want to add below. Include: workflow or role, target users, key tasks, and autonomy constraints.

### Execution Steps

1. **Refresh docs** — Run in parallel: (A) Fetch current Copilot documentation via `Apple Copilot Docs Refresher`, validate mandatory source URLs, repair broken links, produce in-session refresh brief; (B) Fetch Swift Agent Skills via `Apple Swift Skills Reader`, produce in-session domain knowledge brief (required when creating domain-specific skills). Do not persist either brief in the target project.
2. **Analyze** — Scan existing agents, skills, instructions, prompts. Understand conventions the new agent must follow. Identify overlap/gaps. If business-domain context is important, recommend where it should live (concise `copilot-instructions.md` summary, domain-scoped instructions, business domain registry / domain map, or business-domain skill).
3. **Assess** — Current ecosystem shape, overlap risks, integration needs, conductor routing changes if needed.
4. **Clarify only if needed** — Ask minimum targeted questions for missing critical facts.
5. **Generate** — Create new agent with: no role overlap, consistent naming, project-derived prefix, real conventions, supporting files (skill, instruction, prompt, template) when needed. If business complexity warrants it, generate the lightest effective shared business artifact and wire it explicitly into relevant agent inputs/decision rules. Follow YAML frontmatter determinism rules: `agents` uses inline array with display names, `description` is double-quoted, no `tools`/`mcp-servers` unless requested. Include MCP tool preference for external service access.
6. **Audit** — All dimensions + ecosystem coherence. If `REVISE`, fix until `PASS`.
7. **Deliver** — Assessment, agent file(s), integration guide, changes needed to existing agents.

Final quality expectation: zero open findings (including minor) and zero editor diagnostics in generated or updated agent files before completion.

If the repository has unrelated changes, preserve them and continue unless there is a direct file-path conflict.

### Example

> Add a performance profiling agent that helps identify and fix memory leaks, excessive CPU usage, and slow UI rendering in SwiftUI views.

---

## Verify Flow

Point to an existing agent and its associated files (skill, instruction, prompt). The kit will verify consistency, quality, and effectiveness against agent kit standards.

### Execution Steps

1. **Refresh docs** — Run in parallel: (A) Fetch current Copilot documentation via `Apple Copilot Docs Refresher`, validate mandatory source URLs, repair broken links, produce in-session refresh brief; (B) Fetch Swift Agent Skills via `Apple Swift Skills Reader`, produce in-session domain knowledge brief. Do not persist either brief in the target project.
2. **Collect target** — Identify the agent file and all associated files (skill, instruction, prompt, template) the user wants verified. Read every file fully.
3. **Analyze ecosystem** — Scan the rest of the project's agents and conventions to understand the context the target agent operates in.
4. **Evaluate** — Check the target agent and its associated files against the audit rubric:
   - Frontmatter validity and description discoverability
   - Role clarity, mission, non-goals, output contract
   - Scope discipline and `applyTo` narrowness
   - Cross-reference integrity between agent ↔ skill ↔ instruction ↔ prompt
   - Business knowledge persistence fit: if business artifacts exist (domain map/registry, domain-scoped instructions, business-domain skill), verify relevant agents explicitly consume them
   - Naming consistency with project conventions
   - Apple platform specificity where applicable
   - Overlap or conflict with other agents in the ecosystem
   - Behavioral pattern correctness per SKILL.md artifact requirements
5. **Fix** — Propose concrete fixes for every finding. Apply fixes with user approval. Do not skip minor findings.
6. **Audit** — Run full audit on the corrected files. If `REVISE`, fix and re-audit until `PASS`.
7. **Deliver** — Summary of findings, changes made, remaining risks, and ecosystem coherence status.

### Example

> Verify my Swift Testing agent — check if `swift-testing.agent.md`, its skill in `skills/swift-testing/SKILL.md`, and instruction `swift-testing.instructions.md` are consistent and follow kit best practices.
