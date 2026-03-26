# Copilot Documentation Refresh Brief

## Metadata

- Fetch date: 2026-03-21
- Note: This is the kit's persistent snapshot of the last successful official documentation refresh. It is updated in place during kit maintenance. For target project generation, refresh briefs remain in-session only.
- Triggering flow: audit
- Requested workflow: validate and tighten the Agent Builder Kit rules for supporting artifact defaults, dirty-worktree handling, and execution guardrails in generated workflow bundles

## Sources Consulted

### Mandatory Sources
- https://code.visualstudio.com/docs/copilot/customization/custom-agents — confirmed current VS Code custom-agent fields and behavior for `agents`, `handoffs`, `user-invocable`, `disable-model-invocation`, scoped hooks, tool-priority rules, and workspace discovery
- https://docs.github.com/en/copilot/reference/customization-cheat-sheet — confirmed the full primitives overview: instructions (always-on), skills (on-demand), agents (role), prompts (entry points), hooks (gates)
- https://docs.github.com/en/copilot/tutorials/coding-agent/get-the-best-results — confirmed current best-practice emphasis on well-scoped tasks, explicit acceptance criteria, repo-grounded validation guidance, and the role of repository and path-specific instructions
- https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-custom-agents — confirmed the stable conceptual model for custom agents, repository placement, and cross-surface behavior
- https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents — confirmed current creation flow, filename constraints, cross-IDE behavior, and the expectation that agent profiles encode concrete tools and instructions
- https://docs.github.com/en/copilot/reference/custom-agents-configuration — confirmed current frontmatter keys, `infer` retirement, GitHub.com versus IDE differences, tool alias behavior, and MCP processing order
- https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/use-hooks — confirmed `.github/hooks/` placement, `version: 1` expectations, troubleshooting guidance, and debugging practices for hook scripts
- https://docs.github.com/en/copilot/reference/hooks-configuration — confirmed hook event types, input and output behavior, timeout defaults, and that only `deny` is currently processed from customer `preToolUse` output
- https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot — confirmed repository-wide and path-specific instruction behavior, `excludeAgent`, precedence, and support boundaries
- https://docs.github.com/en/copilot/concepts/agents/about-agent-skills — confirmed skills are folders of instructions, scripts, and resources loaded when relevant based on description; multiple focused skills preferred over monolithic ones
- https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-skills — confirmed skill creation flow and the guidance "use skills for more detailed instructions that Copilot should only access when relevant"

### Optional Sources
- none

### Unavailable Sources
- none

## High-Signal Product Facts

### Hard Requirements
- Custom agent files remain Markdown with YAML frontmatter; current important fields include `description`, `name`, `tools`, `target`, `model`, `agents`, `user-invocable`, `disable-model-invocation`, `handoffs`, and `mcp-servers` where supported
- `infer` is retired in current documentation and should not be treated as the preferred control surface
- Path-specific instructions still require narrow `applyTo` globs and can use `excludeAgent` to target coding-agent versus code-review behavior
- Hook files for GitHub Copilot coding agent live under `.github/hooks/`, use `version: 1`, and only `deny` is currently processed from customer `preToolUse` output
- Hook scripts should be deterministic, auditable, and fast; default timeout remains 30 seconds unless overridden

### Best-Practice Guidance
- Best-practice docs continue to push toward well-scoped tasks with explicit acceptance criteria and concrete validation expectations
- Repository-wide instructions should reduce repeated exploration by documenting build, test, validation, and layout facts that agents need frequently
- Path-specific instructions remain the right place for repeated file-type or path-based conventions instead of duplicating those conventions in every agent
- VS Code custom agents should use least-privilege tool sets and explicit subagent or handoff design when sequential workflows matter

### Surface-Specific Caveats
- GitHub.com and IDEs still diverge on some properties; VS Code supports `agents`, `handoffs`, and scoped hooks, while GitHub.com ignores some IDE-only properties
- Tool behavior and availability remain surface-dependent; generator and auditor rules should continue to call out those differences rather than assuming uniform behavior

### Deprecated Or Preview Concerns
- IDE custom-agent behavior outside VS Code remains subject to preview caveats in current GitHub docs
- VS Code scoped hooks remain preview-gated and should not be treated as universally available

## Implications For This Bundle

- The kit should continue to require explicit documentation refresh before generation or major rule updates
- Generated workflow kits should prefer repo-grounded instructions, prompts, validation guidance, and templates over generic prose because current docs reinforce scoped tasks and concrete validation
- For portability across environments, generated agents should avoid frontmatter `tools` and `mcp-servers` by default and encode behavior constraints in instructions unless the user explicitly asks for constrained tool access
- Hook generation must stay conditional and justified; a no-hook rationale is better than speculative hook output when deterministic commands are not available
- The auditor and quick rubric should both check for concrete validation guidance and orchestrator guardrails so lightweight audits do not miss hard requirements

## Conflicts With Repo-Local Assumptions

- The previous kit state allowed a full workflow bundle to pass while generating mostly agents; that no longer matched the documentation emphasis on explicit guidance, repeatable instructions, and validation support
- The previous kit state could overreact to unrelated dirty-worktree changes instead of preserving user edits and continuing safely

## Recommended Follow-Up Changes

- Require supporting skill, instruction, prompt, and template/checklist coverage by default for full workflow generation, unless analysis proves strong existing artifacts already cover those needs
- Require repo-grounded validation commands, micro-change handling, skip rules, and reviewer conflict resolution in generated workflow bundles
- Preserve unrelated worktree changes and escalate only for direct path conflicts with target generated files

## Residual Risks

- Current guidance remains surface-sensitive, especially for IDE preview features and scoped hooks
- Generated instruction quality still depends on analyzer quality; if analysis misses stable scopes or validation commands, downstream artifacts can still be weaker than intended