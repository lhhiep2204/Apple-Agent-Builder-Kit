# Copilot Documentation Refresh Registry

Use this registry before generating or updating any Copilot customization bundle. The goal is to ground output in the latest official GitHub Copilot behavior instead of relying only on repository-local patterns.

## Mandatory Refresh Rule

Before `/generate-workflow-agents`, `/add-agent`, or any direct Agent Builder generation flow:

1. Fetch the current content of every URL in the **Mandatory Source Set**.
2. Validate that each mandatory URL is still reachable and still points to the intended official GitHub or VS Code documentation page.
3. If a mandatory URL is broken, moved, or no longer canonical, locate the current official replacement URL, update this registry before continuing, and record the change in the refresh brief.
2. Extract only the facts that can change generation decisions, such as supported frontmatter, tool behavior, hook behavior, custom instruction precedence, MCP behavior, environment constraints, and newly documented best practices.
3. Produce a short **documentation refresh brief** using `.github/templates/agent-builder/copilot-doc-refresh-brief-template.md` and include:
   - fetch date
   - URLs consulted
   - any repaired or replaced source URLs
   - changed or newly relevant product facts
   - generation constraints or opportunities implied by those facts
4. Use that refresh brief as an in-session input to analysis and generation. Do not persist it as a file in the target project.
5. If any mandatory source is unavailable, continue only if the remaining sources still cover the requested artifact. Explicitly mark the missing source as a risk.

Do not skip this step just because similar files already exist in the repository. A stale or broken registry entry is a maintenance defect and must be repaired when discovered.

## Mandatory Source Set

These sources should be refreshed on every agent-add or workflow-generation run.

| URL | Why it is mandatory |
|-----|----------------------|
| https://code.visualstudio.com/docs/copilot/customization/custom-agents | Authoritative VS Code surface guidance for custom-agent file structure, subagent availability, `agents` frontmatter, handoffs, and editor-specific behavior |
| https://docs.github.com/en/copilot/tutorials/coding-agent/get-the-best-results | Current best practices for task framing, custom instructions, MCP usage, and validation workflow |
| https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-custom-agents | Stable conceptual model for what custom agents are, where they apply, and environment differences |
| https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents | Current creation workflow and practical guidance across GitHub and supported IDEs |
| https://docs.github.com/en/copilot/reference/custom-agents-configuration | Authoritative reference for frontmatter, tools, MCP configuration, and processing rules |
| https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/use-hooks | Current operational guidance for creating, troubleshooting, and debugging hooks |
| https://docs.github.com/en/copilot/reference/hooks-configuration | Authoritative hook schema, event payloads, output behavior, and script best practices |
| https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot | Current behavior for repository instructions, path-specific instructions, precedence, and support boundaries |
| https://docs.github.com/en/copilot/concepts/agents/about-agent-skills | Authoritative conceptual model for skills: what they are, when Copilot loads them, project vs personal scope, and how they differ from instructions |
| https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-skills | Current creation workflow for skills: SKILL.md structure, frontmatter requirements, directory layout, and bundled assets |
| https://docs.github.com/en/copilot/reference/customization-cheat-sheet | Single-page cross-reference of all customization primitives, their usage patterns, and IDE/surface support matrix |

## Optional Source Set

Refresh these when the requested workflow touches the related capability.

| URL | Use when |
|-----|----------|
| https://docs.github.com/en/copilot/customizing-copilot/customizing-the-development-environment-for-copilot-coding-agent | the generated bundle should reason about setup steps, environment constraints, runners, dependencies, or validation in Copilot environments |
| https://docs.github.com/en/copilot/concepts/agents/copilot-memory | deciding how much persistent repo knowledge to encode in instructions versus relying on Copilot Memory |
| https://docs.github.com/en/copilot/tutorials/customization-library/custom-agents | looking for current example patterns after the official rules are understood |

## Excluded As Primary Sources

These may be useful for navigation, but should not be the main source of truth for generation rules.

| URL or source type | Why excluded as primary |
|--------------------|-------------------------|
| https://docs.github.com/en/copilot | too broad; useful as an index, not as the authoritative rule set |
| Community repositories such as awesome-copilot | useful for inspiration, but not authoritative for current product behavior |
| Older blog posts, gists, or forum answers | likely to drift from current product behavior |

## Generation Implications

The refresh brief should be used to catch changes such as:

- newly supported or retired frontmatter keys
- VS Code-specific agent routing, handoffs, and subagent behavior
- changed tool aliases or environment-specific property behavior
- updated instruction precedence or support scope
- hook event behavior or hook script output requirements
- revised guidance on task scoping, validation, or repository onboarding
- new warnings about preview features or IDE support differences

If the refresh brief contradicts repository templates or older generated agents, prefer the official GitHub documentation and update the generated output accordingly.