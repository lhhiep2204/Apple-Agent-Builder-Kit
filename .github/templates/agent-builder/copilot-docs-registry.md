# Copilot Docs Registry

## Metadata

- Source: github/awesome-copilot | Updated: 2026-04-17 | Method: MCP GitHub + web fetch
- Persistent snapshot read by generator at runtime. Maintained by `Apple Copilot Docs Refresher`.

---

## Sources Consulted

### Upstream Source
- `github/awesome-copilot` — Learning Hub articles, example agents/skills/instructions/hooks/plugins/workflows, cookbook entries. Confirmed current frontmatter conventions, agent/skill/instruction/hook architecture, MCP patterns, and surface-specific behavior.
- `llms.txt` is published on the website (`awesome-copilot.github.com/llms.txt`), not in the repository root.

### New Primitives Discovered (since 2026-03-21)
- **Plugins** — Curated bundles of agents and skills organized around themes, workflows, or use cases. Installable via `copilot plugin install <plugin-name>@awesome-copilot`. Browsable in VS Code (`@agentPlugins`, Command Palette → `Chat: Plugins`) and Copilot CLI (`/plugin marketplace browse awesome-copilot`). Awesome Copilot is a default plugin marketplace — no registration required. ~60+ plugins in directory.
- **Agentic Workflows** — AI-powered GitHub Actions automations written in markdown, compiled to `.lock.yml` via `gh aw compile`. Triggered by schedules, events, or slash commands. Requires `gh aw` CLI extension.
- **Copilot SDK cookbook** — Copy-paste-ready recipes for building agentic apps with the GitHub Copilot SDK (cookbook/copilot-sdk/).

## High-Signal Product Facts

### Supported Frontmatter Keys

This is the authoritative allowlist for frontmatter keys the generator may emit. Any key not listed here must not be used — it will cause editor diagnostics or be silently ignored. Updated during kit maintenance when official documentation changes.

| File type | Supported keys |
|-----------|---------------|
| Agent (`.agent.md`) | `name`, `description`, `agents`, `tools`, `model`, `target`, `user-invocable`, `disable-model-invocation`, `mcp-servers`, `handoffs`, `hooks` |
| Prompt (`.prompt.md`) | `description`, `agent` |
| Instruction (`.instructions.md`) | `description`, `applyTo`, `excludeAgent` |
| Skill (`SKILL.md`) | `name`, `description` |
| Plugin (directory-based) | Plugins are directories, not individual files with frontmatter. They bundle agents and skills under a shared README.md. No generator-emittable frontmatter — generation targets agents/skills within plugins, not plugin manifests themselves. |
| Agentic Workflow (`.md` → `.lock.yml`) | Frontmatter keys not yet confirmed. Workflow files compile via `gh aw compile`. **Do not generate workflow files until schema is verified.** |

### Hard Requirements
- Custom agent files remain Markdown with YAML frontmatter; supported keys are listed in the table above
- `infer` is retired in current documentation and should not be treated as the preferred control surface
- Path-specific instructions still require narrow `applyTo` globs and can use `excludeAgent` to target coding-agent versus code-review behavior
- Hook files for GitHub Copilot coding agent live under `.github/hooks/`, use `version: 1`, and only `deny` is currently processed from customer `preToolUse` output
- Hook scripts should be deterministic, auditable, and fast; default timeout remains 30 seconds unless overridden
- Upstream hook examples now include: dependency-license-checker, governance-audit, secrets-scanner, session-auto-commit, session-logger, tool-guardian — confirming the hook surface covers pre/post tool use, session lifecycle, and error handling
- Plugins are installable via `copilot plugin install`; the generator should not produce plugin manifests directly but may generate agents/skills that fit within a plugin structure
- Agentic Workflow files compile to `.lock.yml` and run as GitHub Actions; generator must not emit workflow files until the frontmatter schema is confirmed

### Best-Practice Guidance
- Best-practice docs continue to push toward well-scoped tasks with explicit acceptance criteria and concrete validation expectations
- Repository-wide instructions should reduce repeated exploration by documenting build, test, validation, and layout facts that agents need frequently
- Path-specific instructions remain the right place for repeated file-type or path-based conventions instead of duplicating those conventions in every agent
- VS Code custom agents should use least-privilege tool sets and explicit subagent or handoff design when sequential workflows matter
- Plugins provide a distribution and discovery mechanism for thematically grouped agents + skills; upstream examples show plugins organizing MCP development toolkits, framework migration suites, and language-specific best-practice bundles
- The ecosystem now includes language-specific MCP server development plugins (C#, Go, Java, Kotlin, PHP, Python, Ruby, Rust, Swift, TypeScript) — confirming that MCP server patterns are a first-class Copilot customization concern
- Upstream hook examples demonstrate governance-oriented patterns (license checking, secret scanning, tool access control) alongside productivity patterns (session logging, auto-commit) — reinforcing that hooks serve both safety and workflow automation

### Surface-Specific Caveats
- GitHub.com and IDEs still diverge on some properties; VS Code supports `agents`, `handoffs`, and scoped hooks, while GitHub.com ignores some IDE-only properties
- Tool behavior and availability remain surface-dependent; generator and auditor rules should continue to call out those differences rather than assuming uniform behavior
- Plugin install (`copilot plugin install`) works in both Copilot CLI and VS Code; VS Code also supports `@agentPlugins` search and `Chat: Plugins` command palette
- Agentic Workflows require the `gh aw` CLI extension and compile to GitHub Actions `.lock.yml` — they are a CI/CD surface, not an IDE surface

### Deprecated Or Preview Concerns
- IDE custom-agent behavior outside VS Code remains subject to preview caveats in current GitHub docs
- VS Code scoped hooks remain preview-gated and should not be treated as universally available
- Agentic Workflows (`gh aw compile`) are a new primitive with no confirmed stable frontmatter schema; treat as preview until schema is documented in official product docs
- Plugin marketplace integration appears stable (default marketplace, no registration required) but the plugin manifest format may still evolve

## Implications For Generation

- Prefer repo-grounded instructions, validation guidance, and templates over generic prose
- Avoid frontmatter `tools` and `mcp-servers` by default; encode constraints in instructions unless user explicitly requests
- Hook generation: conditional and justified only. No-hook rationale preferred over speculative hooks
- Do not emit plugin manifests or agentic workflow files until schemas are confirmed
- MCP server generation is viable for Apple platform agents (confirmed by upstream swift-mcp-development plugin)

## Residual Risks

- Agentic workflow frontmatter schema unconfirmed — do not emit workflow files
- Plugin manifest format may evolve — do not generate plugin manifests directly
- `llms.txt` on website (`awesome-copilot.github.com/llms.txt`), not in repo root

---

## Refresh Instructions

1. Fetch `llms.txt` from website (`awesome-copilot.github.com/llms.txt`)
2. Walk linked articles, examples, cookbook entries
3. Extract: frontmatter keys, hard requirements, best practices, caveats, deprecated patterns
4. Update sections in place; update "Last refresh date"
5. If content inaccessible, note under `### Unavailable Content` and keep previous facts
- **Residual Risks** — known gaps or uncertainties after the refresh