# Apple Agent Builder Kit repository guidance

This repository exists to generate and audit high-quality GitHub Copilot customization bundles for Apple-platform engineering workflows.

## Entry Points

Use these prompts to interact with the kit:

- **`/generate-workflow-agents`** — Generate, verify, or improve a full development workflow agent kit. Use this for any project that needs a complete agent bundle (greenfield, established, or mixed project state).
- **`/add-agent`** — Add a new agent to an existing ecosystem, or verify and improve a single existing agent. Use this to extend an ecosystem or audit a specific agent without rebuilding the full kit.

## Primary work surfaces

- `.github/agents/` for custom agents
- `.github/skills/` for reusable workflows and packaged assets
- `.github/instructions/` for path-specific rules
- `.github/prompts/` for user-facing entry points
- `.github/templates/agent-builder/` for scaffolds, decision assets, audit aids, and documentation-refresh templates

## Maintenance Guidelines

When changing generation behavior or templates:
- always refresh the official GitHub Copilot sources listed in `.github/templates/agent-builder/copilot-doc-source-registry.md` before updating generation rules, prompts, or audit criteria
- always refresh the Swift Agent Skills compilation at `https://github.com/twostraws/Swift-Agent-Skills` via `Apple Swift Skills Reader` before updating domain-specific skill content or Apple platform best-practice guidance; update `kit-swift-skills-brief.md` with the result
- prefer current official GitHub documentation over stale repo examples if they conflict
- keep each customization file single-purpose
- keep descriptions concrete and discoverable; they are the discovery surface
- keep `applyTo` narrow and intentional
- do not add hooks, MCP frontmatter restrictions, or extra artifacts unless they materially improve execution quality
- generated agents should leverage MCP tools at runtime when configured — \"avoid MCP in frontmatter\" means do not restrict MCP access, not do not use MCP
- ensure generated agents align to the target project's actual technology stack (Swift version, UI framework, concurrency model), not kit fallback defaults
- derive simulator destinations from project deployment target + Xcode version; never hardcode device models
- when a linter is configured, ensure verify-fix loops use `--strict` flag to catch warnings, not just errors

For this repository, broad guidance belongs here in `copilot-instructions.md`; workflow-specific guidance belongs in skills, prompts, agents, and path-specific instructions.

## Generation Quality Standards

- generated agents must embed the Generation Principles from SKILL.md — these are the non-negotiable behavioral floor
- generated bundles must follow the three-layer context model: broad facts in `copilot-instructions.md`, file-scoped rules in instructions, role-specific workflows in agents
- generated `copilot-instructions.md` must include bundle evolution guidance so teams know when and how to maintain their agents
- business knowledge must be persisted in the lightest effective primitive for the target project (concise `copilot-instructions.md` for simple projects; domain-scoped instructions, domain map/registry, or business-domain skill for business-heavy projects)
- when business artifacts are generated, relevant agents must explicitly consume them in hand-offs and decision rules; no orphaned business context artifacts
- for established projects, the analyzer must check for drift (stale file references, outdated conventions, abandoned patterns) before generation

Before finalizing changes to the kit:
- validate generated or edited customization files against `.github/templates/agent-builder/agent-audit-rubric.md`
- verify the refresh flow, generator flow, and audit flow are consistent with each other
- call out residual risks explicitly instead of claiming certainty the repo has not earned