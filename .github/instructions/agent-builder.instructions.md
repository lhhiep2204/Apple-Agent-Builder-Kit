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

## Avoid

- vague descriptions
- bloated files that combine multiple roles
- duplicated process text across every artifact
- aggressive `applyTo` patterns that waste context
- hook recommendations without a safety rationale
- generic mobile boilerplate in Apple-only bundles