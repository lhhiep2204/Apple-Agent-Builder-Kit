---
description: "Generate, verify, or improve a full Apple-platform development workflow agent kit: analyze the repo, then create, audit, or update agents with quality gating."
agent: "Agent Builder"
---

# Generate Workflow Agents

Analyze the codebase first, classify project state, then act based on what exists:
- **Greenfield** (no agents): generate all agents from scratch
- **Established** (agents exist): run drift detection first (stale file refs, outdated conventions, abandoned patterns), then evaluate and improve existing agents, fill gaps, fix weak collaboration
- **Mixed** (partial setup): preserve working agents, fill gaps

Do not start with a broad interview — extract context by analyzing the codebase directly, then ask only if critical architecture-changing facts remain missing.

## Standard User-Facing Progress

1. Analyze the codebase
2. Assess the project and choose bundle architecture
3. Ask targeted follow-up only if needed
4. Generate or update the bundle
5. Audit and iterate until PASS
6. Deliver the final result

Do not add extra top-level phases for internal setup. Phase 3 auto-skips when no ambiguity remains.

Complete the entire workflow through delivery in this session. When user input is needed, ask with structured options and wait for the response, then continue. Never leave generation incomplete.

## Target Roles

| Role | Responsibility |
|------|---------------|
| Business Analyst | User stories, acceptance criteria, domain rules, edge cases |
| Investigator | As-is/to-be analysis, data flow tracing, impact matrix, investigation reports |
| Implementor | Production code, pre-impl checklist, verify-fix loop |
| Test Specialist | Unit tests (Swift Testing) + UI tests (XCTest), mocks, coverage |
| Code Reviewer | Thorough combined technical + functional review, PR review, severity-driven verdicts |
| Dev Orchestrator | Auto-routes intent, coordinates all stages, iteration loops |

Optionally: project-specific codebase analyzer for large/unfamiliar repos.

## Execution Steps

1. **Analyze** — Deeply scan project: architecture, conventions, domain, existing agents, build setup, testing, delivery workflow. Classify project state. Recommend where shared business knowledge should live: project context instruction only, domain-scoped instructions, a business domain registry / domain map, or a reusable business-domain skill.
2. **Assess** — Summarize: project state, constraints, recommended bundle shape, gaps, workflow-family coverage matrix.
3. **Clarify only if needed** — Ask minimum targeted questions only if a missing fact would materially change architecture.
4. **Generate** — Produce full workflow kit per SKILL.md bundle shape. For established projects: improve weak agents, add missing roles, tighten collaboration contracts. Reference real project conventions. Name generated files with project-derived prefix, while treating `.github/copilot-instructions.md` as the standard non-prefixed workspace-level file: create it if missing, or update it if present while preserving relevant existing content. Also generate `<prefix>-project-context.instructions.md` for deeper reference context. When project business complexity warrants it, generate the appropriate shared business knowledge artifact and wire it into the relevant agents' inputs and decision rules.
5. **Audit** — Full audit including ecosystem coherence. Every finding (major and minor) must be resolved.
6. **Revise** — If `REVISE`, fix every finding and re-audit. Do not accept or skip minor findings. Repeat until `PASS` with zero open issues.
7. **Deliver** — Complete bundle with architecture rationale and residual risks (only genuine uncertainties, not fixable weaknesses).

Final quality expectation: zero open findings (including minor) and zero editor diagnostics in generated agent files before completion.

## Constraints

- Every agent must reference real project conventions, not generic boilerplate
- Project-derived file prefix for generated artifacts
- Full workflow kit includes supporting skills, instructions, prompts, templates per SKILL.md bundle shapes
- Shared business knowledge must be stored in the lightest effective primitive; do not always create a registry/skill and do not leave business-heavy projects with only scattered agent prose
- Bidirectional cross-references between agents and skills; delete intermediate files before finalizing
- Orchestrator defines: business analysis → investigate → implement → test → review sequence
- Orchestrator clarification is non-blocking: offer options with a recommended default and continue provisionally
- Unified Code Reviewer for both technical and functional review
- For established projects: preserve working behavior while fixing weak areas
- Skip full 95% confidence interview — rely on analysis first
- Generated agents do not declare frontmatter `tools` or `mcp-servers` unless explicitly requested — this keeps all tools including MCP servers accessible
- Generated agents that interact with external services (issue trackers, project management, CI/CD) MUST include explicit instructions to prefer MCP tools over URL fetching
- `agents` frontmatter MUST use inline JSON-style array with exact display names: `agents: ["Name A", "Name B"]` — NEVER block list syntax, NEVER filenames
- `description` frontmatter MUST be a double-quoted string with trigger phrases
- All generated files in the same bundle MUST use consistent YAML formatting
