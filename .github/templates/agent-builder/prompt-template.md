---
description: "<Short user-facing entry point for a focused workflow>."
agent: "<Agent display name that should handle this prompt>"
---

# <Prompt Name>

<!-- Scaffolding notes — apply these rules when generating, then remove this block from the output:
- `agent` field MUST reference the agent's exact display name (its `name` frontmatter value)
- NEVER use `mode` — use `agent` for routing
- `description` MUST be a double-quoted string
-->

<Tell the user what to provide and which agent or skill should handle it.>

Routing:
- Handler: <Agent or skill name>
- Ask follow-up questions only when missing facts would materially change the workflow or artifact design

Useful details:
- <Input 1>
- <Input 2>

Example:

> <Example request>