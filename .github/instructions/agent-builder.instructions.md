---
description: "Use when creating or updating Copilot customization files in .github, including Apple platform agents, skills, instructions, prompts, templates, and hook guidance."
applyTo: ".github/**/*.md"
---

# Customization File Standards

Apply these rules when editing Copilot customization files in this repository.

## File Purpose

- Each file should serve one primary purpose
- Prefer composition across agent, skill, instruction, and prompt files instead of one overloaded file
- Do not create extra artifacts unless they improve execution quality or reuse

## Frontmatter

Follow all YAML frontmatter rules in SKILL.md > YAML Frontmatter Generation. Verify keys against the allowlist in `copilot-docs-registry.md`.

Additional rules specific to instruction file editing:
- Use `applyTo` only for guidance that should load automatically; keep narrow
- Consider `excludeAgent` to limit instruction to coding-agent or code-review usage
- Quote `description` values when punctuation could break parsing

## Writing Standard

- Prefer explicit operating rules over broad aspirations
- State goals and non-goals when ambiguity is likely
- Define decision criteria when multiple primitives are possible
- Include validation checklists for complex or failure-prone workflows
- Keep examples realistic and domain-specific
- If the customization targets Apple projects, make platform assumptions and framework defaults explicit
- Prefer Apple-specific terminology over generic mobile phrasing when the scope is Apple-only

## Primitive Selection

Use the matching scaffold template in `.github/templates/agent-builder/`. Replace all `<...>` placeholders with project-specific content.

- Agent: `agent-template.md` | Skill: `skill-template.md` | Instruction: `instruction-template.md` | Prompt: `prompt-template.md` | Hook: `hook-checklist.md`

For primitive selection decisions, apply `primitive-decision-matrix.md`. For role naming, reference `apple-role-catalog.md`.

## Quality Checks

Before finalizing any non-trivial customization file, run the checklist in `.github/templates/agent-builder/agent-audit-rubric.md`. A file passes when it is specific, discoverable, and correctly scoped — not merely readable.

- Description is strong enough to trigger discovery
- Naming is consistent with the folder or file purpose
- Content does not duplicate another file without adding value
- Scope is as narrow as possible without breaking usefulness
- Output contract is clear where the file drives multi-step work
- Analyzer, generator, and auditor roles are split only when the separation adds quality
- Apple-focused files explicitly state platform, framework, concurrency, testing, or lifecycle expectations where needed

## Kit Entry Points

- **`/generate-workflow-agents`** — Generate, verify, or improve a full development workflow agent kit
- **`/add-agent`** — Add a new agent to an existing ecosystem, or verify and improve a single existing agent
- **`/refresh-agents`** — Re-analyze an evolved codebase and surgically update drifted agents, instructions, and skills

## Primary Work Surfaces

- `.github/agents/` for custom agents
- `.github/skills/` for reusable workflows and packaged assets
- `.github/instructions/` for path-specific rules
- `.github/prompts/` for user-facing entry points
- `.github/templates/agent-builder/` for scaffolds, decision assets, audit aids, and kit reference files

## Kit Maintenance Guidelines

When changing generation behavior or templates:
- use `Apple Copilot Docs Refresher` to refresh `copilot-docs-registry.md` and `community-skill-registry.md` from upstream sources (`github/awesome-copilot`, `twostraws/Swift-Agent-Skills`)
- `Apple Copilot Docs Refresher` is a kit-maintenance-only tool — NOT invoked during target project generation
- prefer current upstream content (github/awesome-copilot) over stale repo examples if they conflict
- keep each customization file single-purpose
- keep descriptions concrete and discoverable; they are the discovery surface
- keep `applyTo` narrow and intentional
- do not add hooks, MCP frontmatter restrictions, or extra artifacts unless they materially improve execution quality
- Apple platform guidance must be codebase-first — rules come from target-project evidence, not separate snapshots
- ensure generated agents align to the target project's actual technology stack, not kit fallback defaults

Workflow-specific guidance belongs in SKILL.md, agents, skills, prompts, and path-specific instructions — not in this file.

## Generation Quality Standards

Generated bundles must comply with all artifact requirements and behavioral patterns in SKILL.md. The generator's SKILL.md Compliance Checklist and the auditor's audit rubric verify compliance. Key invariants for anyone editing generated files:

- Generated agents must embed the 8 Generation Principles from SKILL.md as actionable instructions
- Generated agents must follow SKILL.md behavioral patterns: Single-Session Completion Rule, Handoffs Pattern, Generated File Marking, Context Optimization Rules
- Bundle context model: no duplication between layers (per SKILL.md > Context Optimization Rules)

Before finalizing changes to the kit:
- validate generated or edited customization files against `.github/templates/agent-builder/agent-audit-rubric.md`
- verify the generator flow and audit flow are consistent with each other
- call out residual risks explicitly instead of claiming certainty the repo has not earned

## Avoid

- vague descriptions
- bloated files that combine multiple roles
- duplicated process text across every artifact
- aggressive `applyTo` patterns that waste context
- hook recommendations without a safety rationale
- generic mobile boilerplate in Apple-only bundles