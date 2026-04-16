# Apple Role Catalog

Use this as a default menu, not a mandatory output list.

> **Naming rule**: Generated dev workflow agents do NOT use the "Apple" prefix. That prefix is reserved for the Agent Builder Kit's own subagents (`Apple Copilot Docs Refresher`, `Apple Codebase Analyzer`, `Apple Agent Generator`, `Apple Quality Auditor`).

## Core Delivery Roles

- Business Analyst
- Investigator
- Implementor
- Test Specialist
- Dev Orchestrator

## Review Pipeline Roles

- Code Review Orchestrator — routes to sub-reviewers, aggregates verdicts, enforces short-circuit
- Functional Reviewer — business correctness, AC traceability, edge cases
- Technical Reviewer — architecture, performance, security, layer responsibility
- Platform Reviewer (Apple-specific) — memory management, concurrency, SwiftUI, accessibility

### Review Pipeline Scaling

Always generate the full separated review pipeline:
- Code Review Orchestrator — routes to sub-reviewers, aggregates verdicts, enforces short-circuit
- Functional Reviewer — business correctness, AC traceability, edge cases
- Technical Reviewer — architecture, performance, security, layer responsibility
- Platform Reviewer (Apple-specific) — memory management, concurrency, SwiftUI, accessibility (conditionally triggered at runtime when Swift/SwiftUI/UIKit files are changed)

## Spec Pipeline Roles

- Refine-User-Input — intake normalizer (skill, not a full agent)
- Specify-Feature — PRD generation (skill or agent depending on complexity)
- Plan-Implementation — architecture planning (skill)
- Generate-Tasks — task breakdown for execution tracking (skill)

These are typically skills, not standalone agents. Generate as agents only when the project's complexity warrants dedicated personas.

## Ecosystem Roles

- Agent Ecosystem Auditor
- Agent Integration Specialist

## Specialized Quality Roles

- Architect
- Performance And Concurrency Reviewer
- Accessibility And Localization Reviewer
- Release Readiness Reviewer
- Migration And Refactoring Specialist

## Generation Rule

Generate a role only when it changes execution quality materially. Do not create a role just because the catalog makes it available. Every Full Kit bundle includes the full core delivery roles, separated review pipeline, and spec pipeline skills. Specialized quality roles (Architect, Performance Reviewer, etc.) are added when the project's complexity and domain warrant them.
