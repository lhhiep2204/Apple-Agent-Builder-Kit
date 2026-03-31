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
**Validation**: Rules trace back to code or project docs, domain boundaries explicit, cross-domain dependencies called out, no duplication of the full registry in `copilot-instructions.md`

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
