# Apple Codebase Analysis Template

## Project State

- classification: <greenfield | established | mixed>
- evidence:

## Existing Customization Inventory

- repository-wide instructions:
- path-specific instructions:
- prompts:
- skills:
- agents:
- hooks:

## Naming And Discovery Conventions

- file naming pattern:
- agent naming pattern:
- description style:
- trigger phrase quality notes:

## Role Comparison Matrix

| Artifact | Role | Primary responsibilities | Non-goals / boundaries | Quality notes | Overlap risks |
|----------|------|--------------------------|-------------------------|---------------|---------------|
| <file> | <role> | <responsibilities> | <non-goals> | <quality note> | <overlap note> |

## Project Surface

- platforms:
- project type:
- build system:
- targets and schemes:
- package dependencies:
- CI or release workflow:

## Validation And Automation Surface

- local build commands:
- local test commands:
- default simulator destination: <derived from deployment target and Xcode version — never hardcode a device model; use the project config to determine the correct default>
- multi-target or multi-scheme checks:
- simulator or environment constraints:
- deterministic commands safe enough for hook consideration:
- linting tools in use:
- lint tool handling recommendation (instruction vs hook):

## Delivery Workflow Behavioral Signals

- verify-fix commands (build, test, lint) with expected timeouts:
- review methodology depth (diff-only vs context-deep):
- functional/technical review separation:
- investigation methodology (as-is/to-be, impact matrix, sequence diagrams):
- hand-off pattern between investigation and implementation (confirmation checkpoint, structured report, informal):
- team size signal (solo, small agile, larger delivery):

## Recommended Collaboration Topology

- primary workflow lanes:
- default delivery sequence:
- default iteration loops by lane:
- auto-iteration boundaries:
- escalation boundaries back to the user:

## Specialist Hand-Off Contract Candidates

| Upstream role | Downstream role | Required input | Expected output | Pass criteria | Revise criteria | Blocked criteria |
|---------------|-----------------|----------------|-----------------|---------------|-----------------|------------------|
| <role> | <role> | <input> | <output> | <pass> | <revise> | <blocked> |

## Workflow Family Coverage Matrix

| Workflow family | Priority (primary / recurring / optional / omit) | Likely roles | Normal hand-off order | Iteration loop | Escalation boundary | Supporting artifacts |
|-----------------|---------------------------------------------------|--------------|-----------------------|----------------|--------------------|----------------------|
| Planning / story shaping | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Investigation / diagnosis | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Feature delivery | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Testing / validation | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Review | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Ecosystem extension | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Verify / improve existing ecosystem | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |

## Candidate Instruction Scopes

- implementation paths and patterns:
- test paths and patterns:
- UI-specific paths and patterns:
- data layer paths and patterns:
- review or documentation paths and patterns:
- narrow `applyTo` recommendations:
- distinct convention domains that warrant separate instructions:

## Candidate Skill Domains

- delivery workflow skill:
- testing methodology skill (serves which agents):
- investigation methodology skill (serves which agents):
- review methodology skill (serves which agents):
- other domain skill candidates:

## Candidate Prompt Entry Points

- primary entry point (e.g., deliver feature):
- secondary entry points (e.g., investigate bug, generate tests, review code):
- planning or story-shaping entry points:
- ecosystem verification or upgrade entry points:

## Candidate Template And Checklist Assets

- feature delivery packet / handoff checklist:
- investigation report template:
- planning or story-shaping packet:
- validation plan or test packet:
- review packet:
- ecosystem extension or ecosystem improvement checklist:
- other recurring hand-off templates:

## Architecture

- layer model:
- feature structure:
- navigation pattern:
- state management pattern:
- dependency injection pattern:
- data and persistence pattern:

## Apple Technical Defaults And Technology Alignment Profile

- Swift version:
- concurrency model:
- UI frameworks in use:
- testing frameworks:
- accessibility expectations:
- localization expectations:

### Technology Alignment

| Dimension | Project actual | Kit fallback default | Deviation? |
|-----------|---------------|----------------------|------------|
| Swift version | <detected> | Swift 6 | <yes/no> |
| Concurrency model | <detected> | Strict concurrency | <yes/no> |
| UI framework | <detected> | SwiftUI-first | <yes/no> |
| Observation | <detected> | @Observable | <yes/no> |
| Testing framework | <detected> | Swift Testing + XCTest UI | <yes/no> |
| Persistence | <detected> | SwiftData | <yes/no> |
| Migration state | <detected: stable or migrating X→Y> | n/a | <note> |

Generated agents must align to the "Project actual" column. Note deviations explicitly so the generator does not apply kit fallback defaults where the project uses different technology.

## Domain Context

- glossary:
- business rules:
- lifecycle states:
- invariants:

## Recommended Copilot Instructions Content

Content to include in the project's `copilot-instructions.md`:
- project overview:
- architecture summary:
- technology stack and defaults:
- key project-wide conventions:
- domain glossary:
- general coding standards:
- existing `copilot-instructions.md` quality assessment:

## Coverage Gaps And Conflict Risks

- missing role coverage:
- weak or redundant artifacts:
- naming inconsistencies:
- integration risks:

## Recommended Role Families

- <role family>

## Primitive Recommendations

- agents:
- skills:
- instructions:
- prompts:
- hooks:
- supporting templates or checklists:

## Generator Handoff Notes

- preserve:
- improve:
- avoid:
- micro-change lane guidance:
- reviewer conflict handling expectations:

## Risks And Missing Context

- <risk or missing fact>