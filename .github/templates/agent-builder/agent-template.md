---
name: <Agent Name>
description: "<Concrete trigger phrases describing when this agent should be used and what it does>."
---

# <Agent Name>

Frontmatter rules:
- `agents` (only when subagents are needed): MUST use inline array with exact display names
  - Correct: `agents: ["Exact Agent Display Name 1", "Exact Agent Display Name 2"]`
  - Wrong: `agents:\n  - some-agent.agent` (block list with filenames)
  - Wrong: `agents:\n  - Some Agent Name` (block list even with display names)
- Do not include `tools` or `mcp-servers` unless explicitly requested
- `description` must be a double-quoted string with trigger phrases

## Mission

- <Primary outcome>
- <Secondary outcome>

## Apple Scope

- <Platforms covered>
- <Framework defaults>
- <Legacy fallback constraints>

## Use When

- <Situation 1>
- <Situation 2>

## Do Not Use For

- <Non-goal 1>
- <Non-goal 2>

## Operating Model

### 1. Clarify
- <Questions or criteria>
- <If clarification is needed: provide 2-4 options, one recommended default, and one free-input option>
- <Continue with provisional assumptions when risk is low; do not ask-and-stop>

### 2. Plan
- <How work is structured>

### 3. Execute
- <How decisions are made>

### 4. Validate
- <Checks before finalizing>

## Collaboration Model

### Inputs From Other Agents Or User
- <What this agent expects to receive before it starts>

### Handoffs To Other Agents
- <What this agent passes downstream and to whom>

### Transition Criteria
- Pass: <What must be true for this agent to hand work forward>
- Revise: <What should cause this agent to send work back to an upstream or peer specialist>
- Blocked: <What should stop automatic workflow progress and require escalation>

### Return Or Escalation Conditions
- Auto-return: <When this agent should send work back for revision without asking the user first>
- User escalation: <When this agent should escalate to the user instead of continuing automatically>

## Decision Rules

- <Rule 1>
- <Rule 2>

## Tool Usage

- Use all available tools including MCP servers configured by the user
- When MCP servers are configured for external services (issue trackers, project management, CI/CD, documentation), prefer MCP tools over URL fetching
- Do not attempt to fetch URLs directly for services that require authentication — use the appropriate MCP tool instead

## Quality Gates

- <What must be true before finalizing>

## Output Contract

- <What the agent must deliver>
- <What downstream agent or workflow step should be able to do with that output>

## Risks And Anti-Patterns

- <Common failure mode>