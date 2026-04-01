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

- Keep frontmatter minimal and valid YAML
- Quote `description` values when punctuation could break parsing
- Make `description` concrete and searchable with trigger phrases the agent can match
- Use `applyTo` only for guidance that should load automatically
- Keep `applyTo` narrow; avoid broad patterns unless the rule truly applies everywhere in scope
- When authoring path-specific instructions, consider whether `excludeAgent` is needed to limit the instruction to coding-agent or code-review usage
- When authoring custom agents for VS Code workflows, keep surface-specific fields such as `agents`, `handoffs`, `user-invocable`, and `disable-model-invocation` aligned with current official documentation
- `agents` frontmatter MUST use inline JSON-style array with exact display names (`agents: [\"Name A\", \"Name B\"]`), NEVER block list syntax or filenames
- `description` frontmatter MUST be a double-quoted string

## Writing Standard

- Prefer explicit operating rules over broad aspirations
- State goals and non-goals when ambiguity is likely
- Define decision criteria when multiple primitives are possible
- Include validation checklists for complex or failure-prone workflows
- Keep examples realistic and domain-specific
- If the customization targets Apple projects, make platform assumptions and framework defaults explicit
- Prefer Apple-specific terminology over generic mobile phrasing when the scope is Apple-only

## Primitive Selection

When creating a new customization file, start from the matching scaffold template in `.github/templates/agent-builder/`. Replace all placeholder text with project-specific content — never ship template boilerplate.

- Agent: `agent-template.md` — persona, context isolation, multi-stage workflow
- Skill: `skill-template.md` — reusable workflow with steps, templates, or assets
- Instruction: `instruction-template.md` — always-on guidance for matching contexts
- Prompt: `prompt-template.md` — short user-facing entry point
- Hook: `hook-checklist.md` — deterministic enforcement only when necessary and safe

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

## Primary Work Surfaces

- `.github/agents/` for custom agents
- `.github/skills/` for reusable workflows and packaged assets
- `.github/instructions/` for path-specific rules
- `.github/prompts/` for user-facing entry points
- `.github/templates/agent-builder/` for scaffolds, decision assets, audit aids, and kit reference files

## Kit Maintenance Guidelines

When changing generation behavior or templates:
- when updating Copilot product behavior rules, use `Apple Copilot Docs Refresher` to refresh from sources listed in `.github/templates/agent-builder/kit-doc-source-registry.md` and update `kit-doc-refresh.md`
- Apple platform guidance should be codebase-first: strengthen the analyzer, generator, and auditor together so Apple-specific rules come from target-project evidence rather than a separate community snapshot
- `Apple Copilot Docs Refresher` is a kit-maintenance-only tool — it is NOT invoked during target project generation
- prefer current official GitHub documentation over stale repo examples if they conflict
- keep each customization file single-purpose
- keep descriptions concrete and discoverable; they are the discovery surface
- keep `applyTo` narrow and intentional
- do not add hooks, MCP frontmatter restrictions, or extra artifacts unless they materially improve execution quality
- generated agents should leverage MCP tools at runtime when configured — "avoid MCP in frontmatter" means do not restrict MCP access, not do not use MCP
- ensure generated agents align to the target project's actual technology stack (Swift version, UI framework, concurrency model), not kit fallback defaults
- derive simulator destinations from project deployment target + Xcode version; never hardcode device models
- when a linter is configured, ensure verify-fix loops use `--strict` flag to catch warnings, not just errors

Workflow-specific guidance belongs in skills, prompts, agents, and path-specific instructions — not in this file.

## Generation Quality Standards

- generated agents must embed the Generation Principles from SKILL.md — these are the non-negotiable behavioral floor
- generated bundles must follow the context model with a workspace-level foundation: `copilot-instructions.md` for concise global guidance, project context instruction (`applyTo: "**"`) for broad facts, path-scoped instructions for file-pattern rules, and agents for role-specific workflows
- generated bundles must create or update `copilot-instructions.md` for the target project: preserve existing relevant content, add agent ecosystem guide, keep concise (40-80 lines)
- generated project context instruction must include bundle evolution guidance so teams know when and how to maintain their agents
- business knowledge must be persisted in the lightest effective primitive for the target project (concise project context instruction for simple projects; domain-scoped instructions, domain map/registry, or business-domain skill for business-heavy projects)
- when business artifacts are generated, relevant agents must explicitly consume them in hand-offs and decision rules; no orphaned business context artifacts
- for established projects, the analyzer must check for drift (stale file references, outdated conventions, abandoned patterns) before generation

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