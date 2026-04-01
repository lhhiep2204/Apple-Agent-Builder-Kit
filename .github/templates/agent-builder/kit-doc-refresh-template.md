# Copilot Documentation Refresh Brief Template

Use this template during **kit maintenance** when the kit owner runs `Apple Copilot Docs Refresher` to update `kit-doc-refresh.md`. Not used during target project generation.

## Metadata

- Fetch date: <YYYY-MM-DD>
- Triggering flow: kit-maintenance
- Maintenance reason: <e.g., new Copilot feature announced, periodic refresh, suspected drift>
- Note: Do not persist this brief as a file in the target project. When updating the kit itself, write results directly into `kit-doc-refresh.md`.

## Sources Consulted

### Mandatory Sources
- <URL> — <why it mattered>

### Optional Sources
- <URL> — <why it was added>

### Unavailable Sources
- <URL> — <risk created by the missing source>

## High-Signal Product Facts

### Hard Requirements
- <frontmatter, tool, hook, instruction, or environment rule that materially affects generation>

### Best-Practice Guidance
- <best-practice update that should influence prompts, agent roles, or repository guidance>

### Surface-Specific Caveats
- <GitHub.com vs VS Code vs preview differences>

### Deprecated Or Preview Concerns
- <retired properties, preview support, unsupported features>

## Implications For This Bundle

- <what the generator should change>
- <what the auditor must verify>
- <what should move into project context instruction, instructions, skills, prompts, or agents>

## Conflicts With Repo-Local Assumptions

- <template, prompt, agent, or README assumption that is stale or risky>

## Recommended Follow-Up Changes

- <direct file or architecture changes recommended>

## Residual Risks

- <remaining uncertainty after the refresh>