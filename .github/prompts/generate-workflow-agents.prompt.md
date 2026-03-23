---
description: "Generate, verify, or improve a full Apple-platform development workflow agent kit: refresh docs, analyze the repo, then create, audit, or update agents with quality gating."
agent: "Agent Builder"
---

# Generate Workflow Agents

Analyze the codebase first, classify project state, then act based on what exists:
- **Greenfield** (no agents): generate all agents from scratch
- **Established** (agents exist): evaluate and improve existing agents, fill gaps, fix weak collaboration
- **Mixed** (partial setup): preserve working agents, fill gaps

Do not start with a broad interview — extract context by analyzing the codebase directly, then ask only if critical architecture-changing facts remain missing.

## Standard User-Facing Progress

1. Refresh Copilot documentation
2. Analyze the codebase
3. Assess the project and choose bundle architecture
4. Ask targeted follow-up only if needed
5. Generate or update the bundle
6. Audit and iterate until PASS
7. Deliver the final result

Do not add extra top-level phases for internal setup. Phase 4 auto-skips when no ambiguity remains.

## Target Roles

| Role | Responsibility |
|------|---------------|
| Business Analyst | User stories, acceptance criteria, domain rules, edge cases |
| Investigator | As-is/to-be analysis, data flow tracing, impact matrix, investigation reports |
| Implementor | Production code, pre-impl checklist, verify-fix loop |
| Test Specialist | Unit tests (Swift Testing) + UI tests (XCTest), mocks, coverage |
| Technical Reviewer | Architecture, concurrency, performance, dependency-deep review |
| Functional Reviewer | Acceptance criteria, domain rules, edge cases, offline behavior |
| Dev Orchestrator | Auto-routes intent, coordinates all stages, iteration loops |

Optionally: project-specific codebase analyzer for large/unfamiliar repos.

## Execution Steps

1. **Refresh docs** — Fetch current Copilot documentation. Validate mandatory source URLs, repair broken links. Produce in-session refresh brief. Do not persist the brief.
2. **Analyze** — Deeply scan project: architecture, conventions, domain, existing agents, build setup, testing, delivery workflow. Classify project state.
3. **Assess** — Summarize: project state, constraints, recommended bundle shape, gaps, workflow-family coverage matrix.
4. **Clarify only if needed** — Ask minimum targeted questions only if a missing fact would materially change architecture.
5. **Generate** — Produce full workflow kit per SKILL.md bundle shape. For established projects: improve weak agents, add missing roles, tighten collaboration contracts. Reference real project conventions. Apply refresh brief. Name generated files with project-derived prefix.
6. **Audit** — Full audit including ecosystem coherence. Every finding (major and minor) must be resolved.
7. **Revise** — If `REVISE`, fix every finding and re-audit. Do not accept or skip minor findings. Repeat until `PASS` with zero open issues.
8. **Deliver** — Complete bundle with architecture rationale, documentation sources, and residual risks (only genuine uncertainties, not fixable weaknesses).

## Constraints

- Every agent must reference real project conventions, not generic boilerplate
- Refresh official Copilot sources before generation; repair broken URLs
- Project-derived file prefix for generated artifacts; standard filename for `copilot-instructions.md`
- Full workflow kit includes supporting skills, instructions, prompts, templates per SKILL.md bundle shapes
- Bidirectional cross-references between agents and skills; delete intermediate files before finalizing
- Orchestrator defines: business analysis → investigate → implement → test → review sequence
- Separate technical and functional reviewers
- For established projects: preserve working behavior while fixing weak areas
- Skip full 95% confidence interview — rely on analysis first
