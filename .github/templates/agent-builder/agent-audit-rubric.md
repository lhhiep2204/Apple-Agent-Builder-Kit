# Agent Audit Rubric

Quick-reference checklist before finalizing. Full scoring standard and minimums table in `apple-quality-auditor.agent.md`.

## Verdicts

- `PASS`: every finding (major and minor) is resolved. Zero open issues.
- `REVISE`: any finding still open, regardless of severity. All must be fixed before PASS.
- `BLOCKED`: missing information prevents credible validation.

## Quick Checks

0. **Generation Principles**: all 8 principles from SKILL.md encoded in every generated agent? Context optimization rules applied (three-layer model, concise project context instruction, no duplication)? Bundle evolution guidance present in generated project context instruction? No drift indicators (stale file refs, abandoned conventions)?
1. **Discovery**: descriptions concrete, keyword-rich, likely to match real user requests?
2. **Architecture**: each artifact necessary? Conductor/specialist split justified? Bundle complete for workflow? Domain skills created based on multi-agent reuse + strong project signal from analyzer evidence? Domain knowledge for single-agent or thin-signal domains embedded in agent instructions or instruction files rather than separate skills? Business knowledge stored in the lightest shared primitive that fits the project's complexity, rather than always creating a registry/skill or always overloading the project context instruction?
3. **Execution**: constraints explicit? Output contracts clear? Behavioral patterns role-appropriate per SKILL.md? Collaboration lanes defined with hand-offs and iteration loops? Validation guidance repo-grounded? Generated agents avoid frontmatter `tools` and `mcp-servers` unless explicitly requested? Agents that interact with external services include explicit MCP tool preference (prefer MCP over URL fetching)? Build/test commands use project-derived simulator destinations (not hardcoded device names)? Verify-fix loop includes lint with `--strict` when linter is configured? Test strategy defaults to targeted tests and escalates to full-suite only under explicit risk/release criteria? Orchestrator clarification is non-blocking (options + default continuation), not ask-and-stop? Orchestrator includes single-session completion rule (never abandon mid-workflow, ask and wait when blocked)? If a business domain registry, domain map, domain-scoped instruction, or business-domain skill exists, do the relevant agents explicitly consume it?
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

## YAML Frontmatter Validity

- Frontmatter has valid YAML with no editor diagnostics or schema warnings
- `agents` field (if present) MUST use inline JSON-style array with exact agent display names: `agents: ["Name A", "Name B"]` — NEVER block list syntax (`- item`), NEVER filenames like `*.agent.md` or `*.agent`
- `description` field MUST be a double-quoted string
- `name` field MUST be an unquoted string matching the intended display name
- Only documented frontmatter keys used. For agents: `name`, `description`, `agents`, `tools`, `model`, `target`, `user-invocable`, `disable-model-invocation`, `mcp-servers`, `handoffs`, `hooks`. For prompts: `description`, `agent`.
- Prompt files MUST use `agent` field (not `mode`) for routing to a specific agent
- No `tools` or `mcp-servers` unless user explicitly requested constrained tool access
- Generated non-template files contain no unresolved placeholders like `<...>`
- All generated files in the same bundle use consistent YAML formatting conventions

## MCP Tool Usage Guidance

- Agents that interact with external services (issue trackers, project management, CI/CD, documentation) include explicit instructions to prefer MCP tools over URL fetching
- No language that could be misread as "do not use MCP" — the guidance is "do not restrict MCP in frontmatter"

## Pass Rule

Do not pass merely readable bundles. Do not pass with "accepted" minor issues — every finding must be resolved. Pass only when all findings are fixed, the bundle is specific, discoverable, likely to perform well, and cross-reference integrity is clean. Residual risks must be genuine uncertainties (e.g., future platform changes), never fixable weaknesses.
