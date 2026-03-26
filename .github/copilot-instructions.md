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

Before finalizing changes to the kit:
- validate generated or edited customization files against `.github/templates/agent-builder/agent-audit-rubric.md`
- verify the refresh flow, generator flow, and audit flow are consistent with each other
- call out residual risks explicitly instead of claiming certainty the repo has not earned