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

Use these as starting points when generating specific workflow assets. Adapt sections to the project's actual needs.

### Planning / Story Packet

**Produced by**: Business Analyst or Orchestrator planning lane
**Consumed by**: Investigator, Implementor, Test Specialist, Code Reviewer
**Sections**: Problem Statement, Scope and Non-Goals, Acceptance Criteria, Domain Rules and Edge Cases, Implementation Readiness Notes
**Validation**: ACs are testable, scope explicit, downstream has enough context

### Investigation Report

**Produced by**: Investigator
**Consumed by**: Implementor, Orchestrator, Code Reviewer
**Sections**: As-Is State, To-Be State, Change Table (component, change type, file, reason), Impact Matrix (severity), Scenario Mapping
**Validation**: File references concrete, impact severity rated, implementation path clear

### Business Domain Registry / Domain Map

**Produced by**: Business Analyst, Investigator, or Orchestrator planning lane
**Consumed by**: Implementor, Code Reviewer, Test Specialist, Orchestrator
**Sections**: Domain Index, Glossary, Lifecycle States, Key Business Rules, Cross-Domain Dependencies, Source References
**Validation**: Rules trace back to code or project docs, domain boundaries explicit, cross-domain dependencies called out, no duplication of the full registry in the project context instruction

### Validation Plan

**Produced by**: Implementor or Test Specialist
**Consumed by**: Test Specialist, Code Reviewer, Orchestrator
**Sections**: Validation Scope, Test Strategy (unit/integration/UI/manual), Commands and Environments, Risks and Blind Spots
**Validation**: Commands concrete and executable, coverage expectations explicit

### Review Packet

**Produced by**: Implementor or Orchestrator
**Consumed by**: Code Reviewer
**Sections**: Change Summary, Key Files and Dependencies, Validation Evidence, Review Focus Areas
**Validation**: Enough context for deep review (not just diffs), validation evidence present

### Ecosystem Upgrade Checklist

**Produced by**: Analyzer, Generator, or ecosystem governance specialist
**Consumed by**: Auditor, Orchestrator, maintainers
**Sections**: Coverage Improvements, Integration Changes (conductor routing, hand-offs), Conflict Checks, Audit Readiness
**Validation**: No ambiguous or duplicated roles, conductor routing coherent

### Execution Plan (for complex/large tasks)

**Produced by**: Orchestrator planning lane or Investigator
**Consumed by**: Implementor, Test Specialist, Code Reviewer, Orchestrator
**Persisted to**: Session memory (`/memories/session/`) — not checked into the project repo
**Sections**: Goal, Task Decomposition (ordered phases with dependency analysis), Phase Details (files, modules, risk level, validation criteria per phase), Current Status (not-started / in-progress / completed / blocked per phase), Key Decisions Log, Remaining Risks
**Validation**: Each phase independently verifiable (build + tests pass), dependency order correct, scope per phase manageable, progress trackable via `manage_todo_list`

### Repository Impact Map

**Produced by**: Investigator
**Consumed by**: Implementor, Code Reviewer, Orchestrator (for confirmation checkpoint)
**Sections**: Change Summary, File-Level Changes (file path, change description, symbols to modify, existing patterns to follow), Dependency Order (which changes must happen first), Risk Assessment per change group, Human Confirmation Checkpoint
**Validation**: All file paths real and verified, symbol names accurate, dependency order correct, risk levels assigned, structured as tables/lists for agent legibility (not narrative prose)

### Feature Specification (Spec Pipeline)

**Produced by**: Specify-Feature skill (triggered by Orchestrator or user)
**Consumed by**: Plan-Implementation skill, Investigator, Implementor, Test Specialist
**Sections**: Problem Statement, Scope and Non-Goals, Acceptance Criteria (testable Given/When/Then), Domain Rules (with evidence labels), Edge Cases, Dependencies, Technical Constraints, Open Questions
**Validation**: Every AC independently testable, domain rules include evidence type, scope explicit
**Location**: `specs/<feature-id>/spec.md`

### Implementation Plan (Spec Pipeline)

**Produced by**: Plan-Implementation skill
**Consumed by**: Implementor, Orchestrator, Code Review Orchestrator
**Sections**: Architecture Approach, File Changes (with dependency order, symbols, risk), Risk Assessment, Test Strategy, Rollback Plan, Phase -1 Gate Checklist
**Validation**: File changes reference real project files, dependency order correct, Phase -1 gates checked
**Location**: `specs/<feature-id>/plan.md`

### Task Breakdown (Spec Pipeline)

**Produced by**: Generate-Tasks skill
**Consumed by**: Implementor (via `manage_todo_list`), Orchestrator
**Sections**: Ordered task list (description, files, validation criteria, complexity, dependencies), Execution Notes, Validation Checkpoints
**Validation**: Each task independently verifiable, complexity estimated, dependencies correct
**Location**: `specs/<feature-id>/tasks.md`

### Review Scope Document

**Produced by**: Code Review Orchestrator or Orchestrator
**Consumed by**: Functional Reviewer, Technical Reviewer, Platform Reviewer
**Sections**: Change Summary, Blast Radius Assessment, Files Changed (grouped by risk), Spec Reference (if spec pipeline was used), Review Priority Areas, Evidence Baseline (which claims are code-backed vs `[ASSUMPTION]`)
**Validation**: Blast radius assessed, priority areas identified, evidence baseline established
