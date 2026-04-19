# Community Skill Registry

## Metadata

- Source repository: twostraws/Swift-Agent-Skills
- Last refresh date: 2026-04-17
- Refresh method: MCP GitHub + web fetch
- Note: This is the kit's persistent snapshot of the community skill directory. It is updated by `Apple Copilot Docs Refresher` during kit maintenance. The analyzer reads this file as a baseline and enriches with live MCP GitHub data when available at generation time.

## How This File Is Used

1. **Kit maintenance**: The kit owner runs `Apple Copilot Docs Refresher` periodically. The refresher fetches the current state of `twostraws/Swift-Agent-Skills` README.md and all linked sub-repos, then updates this file in place.
2. **Generation runtime**: The analyzer reads this file as a reliable baseline for community skill categories, repos, and content summaries. When MCP GitHub is available, the analyzer fetches live data to override/enrich this snapshot. When MCP is unavailable, this snapshot is the sole community skill source.
3. **Staleness tolerance**: Community skills evolve independently. A snapshot up to 30 days old is acceptable. Beyond that, the kit owner should refresh before major generation runs.

## Registry Structure

Each category lists:
- **Category name** matching the upstream README heading
- **Skills** with repo URL, author, and a content summary (key patterns, anti-patterns, and decision rules extracted from the skill's SKILL.md or primary content file)
- **Content file path** — the path within each sub-repo where the primary skill content lives (typically `<skill-folder>/SKILL.md`)
- **Tech stack match criteria** — what project signals should trigger this category during analysis

---

## Categories

### SwiftUI

**Match when**: UI framework includes SwiftUI (any signal strength)

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| SwiftUI Pro | [twostraws/SwiftUI-Agent-Skill](https://github.com/twostraws/SwiftUI-Agent-Skill) | Paul Hudson | `swiftui-pro/SKILL.md` | Modern SwiftUI patterns, deprecated API avoidance, performance pitfalls, accessibility for buttons/images, navigation patterns, state management, VoiceOver guidance, layout best practices, animation patterns. Targets mistakes LLMs actually make. |
| SwiftUI UI Patterns | [Dimillian/Skills](https://github.com/Dimillian/Skills) | Thomas Ricouard | `swiftui-ui-patterns/SKILL.md` | Best practices and example-driven guidance for building SwiftUI screens and components: navigation, sheets, app wiring, async state, and reusable UI patterns. |
| SwiftUI Design Principles | [arjitj2/swiftui-design-principles](https://github.com/arjitj2/swiftui-design-principles) | Arjit Jaiswal | Check repo for SKILL.md | SwiftUI design principles |
| SwiftUI View Refactor | [Dimillian/Skills](https://github.com/Dimillian/Skills) | Thomas Ricouard | `swiftui-view-refactor/SKILL.md` | Refactors SwiftUI view files toward smaller subviews, MV-style data flow, stable view trees, explicit dependency injection, and correct Observation usage. |

### SwiftData

**Match when**: Persistence includes SwiftData

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| SwiftData Pro | [twostraws/SwiftData-Agent-Skill](https://github.com/twostraws/SwiftData-Agent-Skill) | Paul Hudson | `swiftdata-pro/SKILL.md` | @Model, @Query, predicates, indexes, migrations, relationships, iCloud sync patterns. Targets mistakes LLMs actually make with SwiftData. |
| SwiftData Expert | [vanab/swiftdata-agent-skill](https://github.com/vanab/swiftdata-agent-skill) | Kudrin Dmitry | Check repo for SKILL.md | SwiftData expert patterns |

### Swift Concurrency

**Match when**: Concurrency model is active (async/await, actors, structured concurrency)

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| Swift Concurrency Pro | [twostraws/Swift-Concurrency-Agent-Skill](https://github.com/twostraws/Swift-Concurrency-Agent-Skill) | Paul Hudson | `swift-concurrency-pro/SKILL.md` | async/await, actors, Sendable, task groups, @concurrent, structured concurrency patterns. Covers new features LLMs aren't trained on, edge cases, and common mistakes. |
| Swift Concurrency Expert | [Dimillian/Skills](https://github.com/Dimillian/Skills) | Thomas Ricouard | `swift-concurrency-expert/SKILL.md` | Reviews and fixes Swift 6.2+ concurrency issues: actor isolation problems, Sendable violations, main-actor annotations, and data-race diagnostics. |
| Swift Concurrency Expert | [AvdLee/Swift-Concurrency-Agent-Skill](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill) | Antoine van der Lee | `swift-concurrency/SKILL.md` | Data race diagnostics, async/await conversion, actor isolation, Sendable conformance, Swift 6 migration. Fast-path triage: analyze Package.swift/pbxproj for language mode, strict concurrency, default isolation. Common diagnostics table with smallest safe fixes. Quick fix mode for localized issues. Reference router covers 15+ topics (async-await-basics, tasks, actors, sendable, threading, async-sequences, async-algorithms, testing, performance, memory-management, core-data, migration, linting). Verification checklist enforces build-settings-first approach. |

### Swift Testing

**Match when**: Testing framework includes Swift Testing

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| Swift Testing Pro | [twostraws/Swift-Testing-Agent-Skill](https://github.com/twostraws/Swift-Testing-Agent-Skill) | Paul Hudson | `swift-testing-pro/SKILL.md` | @Test, #expect, #require, parameterized testing, traits, exit tests, confirmations. Targets mistakes LLMs actually make with Swift Testing. |
| Swift Testing Agent Skill | [bocato/swift-testing-agent-skill](https://github.com/bocato/swift-testing-agent-skill) | Eduardo Bocato | Check repo for SKILL.md | Swift Testing patterns |
| Swift Testing Expert | [AvdLee/Swift-Testing-Agent-Skill](https://github.com/AvdLee/Swift-Testing-Agent-Skill) | Antoine van der Lee | `swift-testing-expert/SKILL.md` | Swift Testing expert: #expect/#require macros, traits and tags, parameterized tests, parallel execution and .serialized, async waiting patterns, XCTest migration workflow, test-plan filtering, known issue handling (withKnownIssue), CustomTestStringConvertible for diagnostics. Keeps XCTest for UI automation (XCUIApplication), performance metrics (XCTMetric), ObjC-only. Reference router covers fundamentals, expectations, traits-and-tags, parameterized-testing, parallelization-and-isolation, performance-and-best-practices, async-testing-and-waiting, migration-from-xctest, xcode-workflows. |

### Swift Language

**Match when**: Always relevant (any Swift project)

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| Swift API Design Guidelines | [Erikote04/Swift-API-Design-Guidelines-Agent-Skill](https://github.com/Erikote04/Swift-API-Design-Guidelines-Agent-Skill) | Erik Sebastián de Erice | Check repo for SKILL.md | Swift API naming conventions, design guidelines |
| Swift FormatStyle Skill | [n0an/Swift-FormatStyle-Agent-Skill](https://github.com/n0an/Swift-FormatStyle-Agent-Skill) | Anton Novoselov | Check repo for SKILL.md | Swift FormatStyle patterns |

### Accessibility

**Match when**: Accessibility signal is moderate or strong, or project targets public distribution

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| iOS Accessibility Agent Skill | [dadederk/iOS-Accessibility-Agent-Skill](https://github.com/dadederk/iOS-Accessibility-Agent-Skill) | Daniel Devesa | Check repo for SKILL.md | iOS accessibility patterns and VoiceOver guidance |
| Swift Accessibility Skill | [PasqualeVittoriosi/swift-accessibility-skill](https://github.com/PasqualeVittoriosi/swift-accessibility-skill) | Pasquale Vittoriosi | Check repo for SKILL.md | Swift accessibility patterns |
| Apple Accessibility Skills | [rgmez/apple-accessibility-skills](https://github.com/rgmez/apple-accessibility-skills) | Roberto Gómez Muñoz | Check repo for SKILL.md | Apple platform accessibility patterns |

### App Store

**Match when**: Project targets App Store distribution, or App Store Connect integration detected

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| App Store Connect CLI | [rudrankriyam/app-store-connect-cli-skills](https://github.com/rudrankriyam/app-store-connect-cli-skills) | Rudrank Riyam | Check repo for SKILL.md | App Store Connect CLI integration patterns |
| App Store Changelog | [Dimillian/Skills](https://github.com/Dimillian/Skills) | Thomas Ricouard | `app-store-changelog/SKILL.md` | Creates user-facing App Store release notes from git history: collects changes since last tag, filters for user-visible work, rewrites into concise "What's New" bullets. |
| App Store ASO Optimization | [timbroddin/app-store-aso-skill](https://github.com/timbroddin/app-store-aso-skill) | Tim Broddin | Check repo for SKILL.md | App Store optimization patterns |

### Architecture

**Match when**: Always relevant for non-trivial projects

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| Swift Architecture Skill | [efremidze/swift-architecture-skill](https://github.com/efremidze/swift-architecture-skill) | Lasha Efremidze | Check repo for SKILL.md | Swift architecture patterns |

### Core Data

**Match when**: Persistence includes Core Data

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| Core Data Expert | [AvdLee/Core-Data-Agent-Skill](https://github.com/AvdLee/Core-Data-Agent-Skill) | Antoine van der Lee | `core-data-expert/SKILL.md` | Core Data expert: stack setup, fetch requests, NSFetchedResultsController, saving/merge conflicts, threading with Swift Concurrency, batch operations with persistent history tracking, migrations (lightweight/staged/deferred iOS 17+), CloudKit sync (NSPersistentCloudKitContainer). Key rules: never pass NSManagedObject across contexts (use NSManagedObjectID), verify persistent history tracking for batch ops, Production CloudKit schema is immutable. Reference router covers 14 topics. |

### Focus Management

**Match when**: Project targets tvOS, or focus management APIs detected

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| Swift FocusEngine Pro | [mhaviv/Swift-FocusEngine-Agent-Skill](https://github.com/mhaviv/Swift-FocusEngine-Agent-Skill) | Michael Haviv | Check repo for SKILL.md | tvOS/macOS focus engine patterns |

### Performance

**Match when**: Performance signal is moderate or strong, or SwiftUI performance concerns detected

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| SwiftUI Performance Audit | [Dimillian/Skills](https://github.com/Dimillian/Skills) | Thomas Ricouard | `swiftui-performance-audit/SKILL.md` | Audits SwiftUI runtime performance from code and architecture: invalidation storms, identity churn, layout thrash, heavy render work, and profiling guidance with Instruments. |

### Security

**Match when**: Security/privacy signal is moderate or strong, or financial/health/enterprise app detected

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| Swift Security Expert | [ivan-magda/swift-security-skill](https://github.com/ivan-magda/swift-security-skill) | Ivan Magda | Check repo for SKILL.md | Swift security patterns, keychain, encryption, auth |

### Tool Skills

**Match when**: Project uses simulator workflows, or Figma-to-SwiftUI pipeline detected

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| iOS Simulator Skill | [conorluddy/ios-simulator-skill](https://github.com/conorluddy/ios-simulator-skill) | Conor Luddy | Check repo for SKILL.md | iOS Simulator automation patterns |
| Figma to SwiftUI Skill | [daetojemax/figma-to-swiftui-skill](https://github.com/daetojemax/figma-to-swiftui-skill) | Ermolaev Maxim | Check repo for SKILL.md | Figma design-to-SwiftUI conversion |

### User Interface

**Match when**: UI writing/copywriting quality is a concern, or writing-intensive UI detected

| Skill | Repo | Author | Content path | Key knowledge |
|-------|------|--------|-------------|---------------|
| Writing for Interfaces Skill | [andrewgleave/skills/tree/main/writing-for-interfaces](https://github.com/andrewgleave/skills/tree/main/writing-for-interfaces) | Andrew Gleave | `writing-for-interfaces/SKILL.md` | Interface writing patterns and UX copy |

---

## Related Resources

These are additional references listed in the upstream directory:

| Resource | URL | Type |
|----------|-----|------|
| SwiftAgents (AGENTS.md) | [twostraws/SwiftAgents](https://github.com/twostraws/SwiftAgents) | AGENTS.md / CLAUDE.md file for Swift projects |
| Xcode 26 System Prompts | [artemnovichkov/xcode-26-system-prompts](https://github.com/artemnovichkov/xcode-26-system-prompts) | Xcode system prompt reference |
| Awesome iOS AI Skills | [Techopolis/awesome-ios-ai](https://github.com/Techopolis/awesome-ios-ai) | Alternative curated list of iOS AI skills |
| Build Your Own Skill Guide | [build.ms/2025/10/17/your-first-claude-skill](https://build.ms/2025/10/17/your-first-claude-skill) | Tutorial for creating custom skills |

---

## Refresh Instructions

When updating this file during kit maintenance:

1. Fetch the current README.md of `twostraws/Swift-Agent-Skills` via MCP GitHub or web fetch
2. Compare the current category listing and skill links against this snapshot
3. For new skills: add them to the appropriate category with repo URL, author, and content path
4. For removed skills: remove the entry from the appropriate category
5. For each skill with "Check repo for SKILL.md" in the content path column: attempt to read the repo to discover the actual SKILL.md path and extract key knowledge. Update the content path and key knowledge columns
6. For each skill where the key knowledge is already populated: verify the content is still accurate by reading the sub-repo's primary content file. Update if content has changed materially
7. Check the "Related Resources" section for new entries in the upstream README
8. Update the "Last refresh date" in the metadata section
9. If any sub-repo is inaccessible, mark the skill row with `[UNREACHABLE — <date>]` and keep the previous content
