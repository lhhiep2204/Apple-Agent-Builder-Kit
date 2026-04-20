# Agent Audit Rubric

Quick-reference checklist before finalizing. Full scoring standard and minimums table in `apple-quality-auditor.agent.md`.

## Verdicts

PASS = zero open findings (major and minor all resolved). REVISE = any finding open. BLOCKED = missing info. Do not accept or defer minor issues. Residual risks must be genuine uncertainties, not fixable weaknesses. Full scoring standard in `apple-quality-auditor.agent.md`.

## Quick Checks

0. **Generation Principles**: all 8 principles from SKILL.md encoded in every generated agent? Principle #5 (clarify before acting) explicitly requires `vscode_askQuestions`? Context optimization rules applied (three-layer model, concise project context instruction, no duplication)? Bundle evolution guidance present in generated project context instruction including steering loop and entropy management? No drift indicators (stale file refs, abandoned conventions)?
1. **Discovery**: descriptions concrete, keyword-rich, likely to match real user requests?
2. **Architecture**: each artifact necessary? Conductor/specialist split justified? Bundle complete for workflow? Domain skills created based on multi-agent reuse + strong project signal from analyzer evidence? Domain knowledge for single-agent or thin-signal domains embedded in agent instructions or instruction files rather than separate skills? Business knowledge stored in the lightest shared primitive that fits the project's complexity, rather than always creating a registry/skill or always overloading the project context instruction?
3. **Execution**: constraints explicit? Output contracts clear? Behavioral patterns role-appropriate per SKILL.md? Collaboration lanes with hand-offs and iteration loops? Validation repo-grounded? No frontmatter `tools`/`mcp-servers` unless requested? MCP tool preference for external services? Build/test commands use project-derived simulator destinations? Verify-fix loop includes lint `--strict` when linter configured? Tests default to targeted scope, escalate to full-suite only under explicit criteria? Orchestrator uses `vscode_askQuestions` (non-blocking, options + default)? Single-session completion rule present? Planning lane for complex tasks? Context persistence (session memory + `manage_todo_list` + compaction)? Business domain artifacts explicitly consumed by relevant agents? Harness engineering aligned (feedforward/feedback, structured investigation, agent legibility, steering loop, entropy management)?
4. **Apple Specificity**: platforms, frameworks, concurrency, testing, accessibility, localization, capabilities, and lifecycle explicit? Generated agents aligned to the project's actual technology profile and analyzer evidence, not kit fallback defaults or generic Apple assumptions?
5. **Scope Discipline**: files single-purpose? `applyTo` narrow? No drift risk?
6. **Ecosystem Coherence**: no naming conflicts? No role overlap? Existing agents evaluated?
7. **Context Efficiency**: outputs concise and phase-bounded? no repeated long context dumps? hand-offs prefer delta summaries + file references over duplicated prose?

## Cross-Reference Integrity

- Every template referenced by ≥1 agent/skill/prompt (no orphans)
- Every skill referenced by ≥1 agent with bidirectional linking (no orphans)
- Delivery workflow skill present (mandatory for full kits)
- Business-domain artifacts, if generated, are referenced by the agents that need them (no orphaned business context)
- No intermediate files left in target project
- `copilot-instructions.md` present (created or updated) with workspace-level content: concise project overview, tech stack, conventions, agent ecosystem guide, cross-references. Not duplicating `<prefix>-project-context.instructions.md` content. When the target project had an existing file, relevant prior content preserved.
- Project context instruction present with `applyTo: "**"`
- Constitution present with `applyTo: "**"` — all agents acknowledge it
- Spec pipeline skills present — wired into orchestrator

### Template Orphan Check

- Every file in `.github/templates/<prefix>/` is referenced by ≥1 generated skill or agent by explicit path
- `spec-template.md` → referenced by Specify-Feature skill
- `plan-template.md` → referenced by Plan-Implementation skill
- `tasks-template.md` → referenced by Generate-Tasks skill
- `investigation-report-template.md` (if generated) → referenced by Investigator agent or Investigation skill
- **FAIL if**: any template exists in `.github/templates/<prefix>/` but has zero references from skills or agents

### Workflow Asset Orphan Check

- Every generated workflow asset has both a producer (agent/skill that creates it) and a consumer (agent/skill that reads it) that reference it by path
- **FAIL if**: any workflow asset has zero inbound references

## Constitution Checks

Full constitution pattern in SKILL.md > Constitution Pattern. Template in `constitution-template.md`. Verify:
- `<prefix>-constitution.md` exists as an instruction file with `applyTo: "**"`
- Articles reference actual project conventions, not generic boilerplate
- Phase -1 gates present (Simplicity, Duplication, Business Logic, Impact)
- Evidence standard (Article II) aligns with agent evidence labeling behavior
- All generated agents include a note acknowledging the constitution

## Spec-Driven Pipeline Checks

Full pipeline specification in SKILL.md > Spec-Driven Pipeline. Template wiring rules in SKILL.md > Template Cross-Reference Requirements. Verify:
- Refine-User-Input skill normalizes raw requests into structured format
- Specify-Feature skill produces specs in `specs/<feature-id>/spec.md`
- Plan-Implementation skill produces plans in `specs/<feature-id>/plan.md`
- Generate-Tasks skill produces task breakdowns in `specs/<feature-id>/tasks.md`
- Customized templates generated for the target project
- Each spec pipeline skill explicitly references its corresponding template by path (see Template Orphan Check above)
- Orchestrator activates pipeline for non-trivial features (>3 files, cross-module, business logic)
- Trivial changes (single-file fixes, typos, config edits) skip the spec pipeline

## Separated Review Pipeline Checks

Full pipeline specification in SKILL.md > Separated Review Pipeline. Verify:
- Full 4-agent review pipeline generated: Code Review Orchestrator + Functional Reviewer + Technical Reviewer + Platform Reviewer
- Platform Reviewer is conditionally triggered at runtime (only when Swift/SwiftUI/UIKit files changed)
- Short-circuit rule: Functional Reviewer BLOCKER → REJECT immediately, skip remaining stages
- Every review finding includes: code anchor, evidence type, severity, fix suggestion
- Code Review Orchestrator routes to sub-reviewers in correct order and aggregates verdicts

## Handoffs Checks

Full specification in SKILL.md > Handoffs Pattern. Verify:
- Every agent that delegates to other agents has `handoffs:` in YAML frontmatter
- Handoffs array includes all agents the delegator routes to
- Each handoff has: `agent` (exact display name), `label` (short action text), `prompt` (contextual)
- Orchestrators have handoffs to all specialist agents
- Specialist agents have handoffs back to orchestrator for completion/escalation
- Agents that never delegate (leaf specialists) do NOT have handoffs

## Evidence Standard Checks

Labels and enforcement rules per SKILL.md > Evidence Standard. Verify:
- All agents define evidence labels: code/doc-backed (no label), `[USER-CONFIRMED]`, `[ASSUMPTION]`, `[NEEDS CLARIFICATION]`
- Investigators mark uncertain findings as `[ASSUMPTION]`
- Implementors verify `[ASSUMPTION]` before coding
- Reviewers challenge unresolved `[ASSUMPTION]` labels
- Orchestrators track assumption resolution across phases
- Constitution Article II (if present) codifies the standard

## Bundle Completeness Checks

Full minimums table in `apple-quality-auditor.agent.md`. Bundle shapes in SKILL.md > Bundle Shapes. Verify:
- Bundle includes all components from the Full Workflow Kit Minimums table
- No components omitted without explicit justification
- Bundle is complete regardless of project size — agents adapt behavior to context, but no features are withheld

## Hooks Checks

Full hook generation rules in SKILL.md > Hooks Generation. Validation criteria in `hook-checklist.md`. Verify:
- When SwiftFormat/SwiftLint detected: hook generated or explicit skip rationale provided
- Hook files include explanatory comments
- Hooks use analyzer-detected scheme/destination for compile checks
- Hooks validated against `hook-checklist.md` three-condition test

## Generated File Marking Checks

Full rules in SKILL.md > Generated File Marking. Verify:
- Every generated file includes `<!-- Generated by Apple Agent Builder Kit -->` immediately after the YAML frontmatter closing `---`
- Marker is NOT placed before frontmatter (that breaks YAML parsing and causes editor diagnostics)
- Template files within the kit itself do NOT have markers — only target project files
- The kit uses this exact marker pattern to identify generated files during refresh and maintenance

## Cross-Session Persistence Checks

Full rules in SKILL.md > Cross-Session Persistence and SKILL.md > Large Task Execution Pattern. Verify:
- Generated orchestrator includes cross-session persistence guidance for large tasks
- Guidance covers: repo memory for durable state, persistent project files for migration tracking
- Progress summary format is specified (completed phases, current status, remaining work, key decisions)
- Session vs repo memory distinction is clear (session memory = within-session only; repo memory = across sessions)

## Community Skill Coverage Checks

Full integration rules in SKILL.md > Community Skill Discovery. Generator-specific implementation in `apple-agent-generator.agent.md` > Community Skill Integration Rule. Verify:
- If analyzer reported community skill discovery results, every matched category must be either consumed by at least one generated agent or documented as intentionally skipped with rationale
- Extracted community skill knowledge embedded in relevant agents as specific, actionable rules — not just "follow SwiftUI best practices" but concrete patterns, anti-patterns, and corrections
- When multiple skills match the same tech area (e.g., two SwiftUI skills from different authors), the generator must either merge non-conflicting knowledge or choose the best fit and note the alternative
- Community skill recommendations included in generated project context instruction or relevant agent instructions
- Agents that consume community skill knowledge include a `## Community Skill Knowledge` attribution section listing sources
- If community skill registry was the only source (no live MCP enrichment), agents note this as `[REGISTRY-ONLY]` — knowledge may be stale
- Deep-crawl results (actual SKILL.md content) preferred over surface-level README summaries — if only README was available, this is noted
- Community skill knowledge routed to the correct agent roles: SwiftUI → implementor + reviewer, Testing → test specialist, Concurrency → implementor + reviewer, Security → reviewer + investigator, Architecture → orchestrator + investigator, Accessibility → implementor + reviewer, Performance → reviewer + investigator

## YAML Frontmatter Validity

Full rules in SKILL.md > YAML Frontmatter Generation. Allowlist in `copilot-docs-registry.md` > Supported Frontmatter Keys. Verify:
- Frontmatter has valid YAML with no editor diagnostics or schema warnings
- `agents` field (if present) MUST use inline JSON-style array with exact agent display names: `agents: ["Name A", "Name B"]` — NEVER block list syntax (`- item`), NEVER filenames like `*.agent.md` or `*.agent`
- `description` field MUST be a double-quoted string
- `name` field MUST be an unquoted string matching the intended display name
- Only frontmatter keys listed in the "Supported Frontmatter Keys" table in `copilot-docs-registry.md` are used — no unlisted, undocumented, or deprecated keys
- Prompt files MUST use `agent` field (not `mode`) for routing to a specific agent
- No `tools` or `mcp-servers` unless user explicitly requested constrained tool access
- Generated non-template files contain no unresolved placeholders like `<...>`
- All generated files in the same bundle use consistent YAML formatting conventions

## MCP Tool Usage Guidance

Full rules in `apple-agent-generator.agent.md` > Tool and MCP Usage Guidance. Verify:
- Agents that interact with external services (issue trackers, project management, CI/CD, documentation) include explicit instructions to prefer MCP tools over URL fetching
- No language that could be misread as "do not use MCP" — the guidance is "do not restrict MCP in frontmatter"

## Clarification Tool Usage

Per SKILL.md > Single-Session Completion Rule. Verify:
- All generated agents use `vscode_askQuestions` for structured clarification — never plain-text questions that end the session
- Orchestrator clarification uses `vscode_askQuestions` with 2-4 options, recommended default, and free-input option
- Principle #5 (clarify before acting on ambiguity) explicitly references `vscode_askQuestions`

## Harness Engineering Alignment

Full specification in SKILL.md > Harness Engineering Alignment. Verify:
- Feedforward (guides) and feedback (sensors) controls are distinguished in the agent ecosystem
- Computational controls (linters, type checkers, tests) and inferential controls (AI review) are identified and leveraged appropriately
- Investigator output produces a structured repository impact map (real file paths, symbol names, dependency-ordered change groups) — not narrative prose
- Inter-agent outputs are optimized for agent legibility (structured tables/lists, not paragraphs)
- Bundle evolution guidance includes steering loop (improve harness when issues recur) and entropy management
- Orchestrator includes planning lane for complex tasks with decomposition, plan persistence, and chunked execution
- Context persistence strategy present: session memory for investigation/plans, `manage_todo_list` for progress, context compaction for long tasks
