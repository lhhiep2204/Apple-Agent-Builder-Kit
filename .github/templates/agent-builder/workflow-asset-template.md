# Workflow Asset Template

Use this when generating a reusable project-specific template, checklist, or hand-off document for a workflow kit.

## Purpose

- <What recurring hand-off or validation moment this asset supports>
- <Which agents, prompts, or skills should use it>

## Workflow Integration

- Produced by: <Which agent, prompt, or skill fills this out>
- Consumed by: <Which downstream agent or workflow step depends on it>
- Handoff trigger: <When this asset should be created and passed along>

## Use When

- <Situation 1>
- <Situation 2>

## Required Inputs

- <Input 1>
- <Input 2>

## Template Structure

### Context
- <Project or feature context>

### Required Sections
- <Section 1>
- <Section 2>

### Validation
- <What must be checked before this asset is considered complete>

## Avoid

- <Anti-pattern 1>
- <Anti-pattern 2>

---

## Common Workflow Asset Patterns

Adapt to the project's actual needs. Each pattern lists producer → consumer and key sections.

| Pattern | Producer | Consumer | Key Sections |
|---------|----------|----------|-------------|
| Planning / Story Packet | BA or Orchestrator | Investigator, Implementor, Test Specialist, Reviewer | Problem, Scope/Non-Goals, ACs, Domain Rules, Edge Cases |
| Investigation Report | Investigator | Implementor, Orchestrator, Reviewer | As-Is, To-Be, Change Table, Impact Matrix, Scenario Map |
| Business Domain Registry | BA, Investigator, or Orchestrator | Implementor, Reviewer, Test Specialist, Orchestrator | Domain Index, Glossary, Lifecycle States, Business Rules, Dependencies |
| Validation Plan | Implementor or Test Specialist | Test Specialist, Reviewer, Orchestrator | Scope, Test Strategy, Commands/Environments, Risks |
| Review Packet | Implementor or Orchestrator | Code Reviewer | Change Summary, Key Files, Validation Evidence, Focus Areas |
| Ecosystem Upgrade Checklist | Analyzer, Generator | Auditor, Orchestrator | Coverage, Integration Changes, Conflict Checks, Audit Readiness |
| Execution Plan | Orchestrator or Investigator | Implementor, Test Specialist, Orchestrator | Goal, Task Decomposition, Phase Details, Status, Decisions, Risks. Persisted to session memory. |
| Repository Impact Map | Investigator | Implementor, Reviewer, Orchestrator | Change Summary, File-Level Changes (path/symbols/patterns), Dependency Order, Risk per group. Structured as tables for agent legibility. |
| Feature Spec (Spec Pipeline) | Specify-Feature skill | Plan skill, Investigator, Implementor, Test Specialist | Problem, Scope, ACs (Given/When/Then), Domain Rules, Edge Cases. Location: `specs/<id>/spec.md` |
| Implementation Plan (Spec Pipeline) | Plan-Implementation skill | Implementor, Orchestrator, Review Orchestrator | Architecture, File Changes (dependency order), Risk, Test Strategy, Phase -1 Gates. Location: `specs/<id>/plan.md` |
| Task Breakdown (Spec Pipeline) | Generate-Tasks skill | Implementor, Orchestrator | Ordered tasks (files, validation, complexity, dependencies). Location: `specs/<id>/tasks.md` |
| Review Scope Document | Review Orchestrator | Functional/Technical/Platform Reviewer | Change Summary, Blast Radius, Files by Risk, Spec Reference, Evidence Baseline |

---

## Cross-Reference Wiring Rule

Every generated workflow asset must have ≥1 producer and ≥1 consumer referencing it by path. Orphan check in `agent-audit-rubric.md`. Generator must add path references in both producer output and consumer input.
