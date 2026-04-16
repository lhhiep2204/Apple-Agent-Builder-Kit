# User Playbook Template

Use this when generating a user playbook doc for a target project's agent ecosystem. The playbook helps users understand how to work with the generated agents.

---

## Template

```markdown
# <Project Name> Agent Playbook

## Quick Start

| What you want to do | How to do it |
|---------------------|-------------|
| Start a new feature | `/deliver-feature` or ask the Dev Orchestrator |
| Investigate existing code | Ask the Investigator to analyze a module |
| Write tests | Ask the Test Specialist |
| Review code | Ask the Code Review Orchestrator |
| Add a new spec | Use the Specify-Feature skill |

## Available Agents

<List all generated agents with one-line description and when to use each>

| Agent | When to use |
|-------|-------------|
| <Dev Orchestrator> | Any development task — it routes to the right specialist |
| <Implementor> | Direct implementation when you know exactly what to change |
| <Test Specialist> | Generate or update tests |
| <Code Review Orchestrator> | Review changes before merge |
| <Investigator> | Understand existing code before making changes |
| <Business Analyst> | Clarify requirements, write user stories |

## Common Workflows

### Feature Development (Full Pipeline)
1. Describe the feature to the Dev Orchestrator
2. (If spec pipeline active) Orchestrator produces spec → plan → tasks
3. Investigator analyzes impact
4. Implementor executes tasks
5. Test Specialist generates tests
6. Review pipeline validates changes

### Quick Fix
1. Describe the bug to the Dev Orchestrator
2. Orchestrator routes to Implementor (micro-change lane for small fixes)
3. Implementor fixes and runs verify-fix loop
4. Quick review pass

### Code Review
1. Ask the Code Review Orchestrator to review your changes
2. Functional Review runs first (business correctness)
3. If no blockers: Technical Review + Platform Review
4. Combined verdict with actionable findings

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Agent gives generic advice | Provide more context: file paths, module names, specific behavior |
| Agent references wrong files | Agents may have stale references — check if files were renamed/moved recently |
| Review is too strict/lenient | Adjust the review scope: specify which files to focus on |
| Agent stops mid-task | Re-invoke with context from session memory; progress is tracked in manage_todo_list |
```

---

## Generation Rules

- Replace all `<placeholders>` with actual generated agent names and project prompts
- Only include agents that were actually generated (do not list roles that were skipped)
- Workflow descriptions must match the actual pipeline configuration
- Keep practical and concise — this is an operating manual, not a reference doc
