# Swift Platform Skills Brief

## Metadata

- Fetch date: 2026-03-28
- Note: This is the kit's persistent snapshot of the last successful Swift Agent Skills fetch. It is updated in place during kit maintenance by `Apple Swift Skills Reader`. For target project generation, this brief should be refreshed in-session.
- **Capture depth: README-based** — this snapshot was distilled from repo landing pages (README.md). Direct `SKILL.md` + `references/*.md` content was not captured because community repos use subdirectory layout (`<skill-name>/SKILL.md`), and raw fetches of subdirectory files were not performed. In-session runs of `Apple Swift Skills Reader` should fetch actual SKILL.md + references content for higher-fidelity domain knowledge.
- Compilation page: https://github.com/twostraws/Swift-Agent-Skills
- Repos fetched: 17 of 17
- Skills counted: 22 (some repos contain multiple sub-skills)
- Warnings: 0 — all repos are MIT-licensed and publicly accessible

---

## Domain Knowledge

### 1. SwiftUI

**Signal strength:** VERY HIGH — 6 distinct skills from 5 authors; >2k combined stars

**Primary sources:**
- `twostraws/SwiftUI-Agent-Skill` (subdirectory `swiftui-pro/`) — iOS 26+, Swift 6.2+. Explicitly targets mistakes LLMs actually make: invisible VoiceOver buttons, deprecated API usage, layout surprise bugs, state management pitfalls.
- `Dimillian/Skills` → `swiftui-ui-patterns/` + `swiftui-view-refactor/` + `swiftui-performance-audit/` + `swiftui-liquid-glass/` (2.4k stars multi-skill repo)
- `arjitj2/swiftui-design-principles` (5 stars) — base-4/8 spacing grid, weight-based typography hierarchy, semantic colors over hardcoded hex, WidgetKit native components (Gauge vs manual drawing), NavigationStack over bare ZStack
- `daetojemax/figma-to-swiftui-skill` (8 stars) — Figma MCP → native SwiftUI translation; treats MCP output as spec not code; never ports React+Tailwind patterns

**Key topics:**
- Navigation: `NavigationStack` + `NavigationSplitView` (never bare ZStack for nav)
- Layout: `VStack`/`HStack`/`LazyVGrid` with spacing system; avoid arbitrary padding values
- State: `@State` / `@Binding` / `@Observable` / `@Environment` boundaries; avoid unnecessary `@StateObject` in nested views
- Liquid Glass (iOS 26+): `GlassEffect`, `glassEffect()` modifier, `VisualEffect` — only on iOS 26+, guard with `#available(iOS 26, *)`
- Performance: avoid identity-breaking modifiers in lists, `LazyVStack` vs `VStack` tradeoffs, `.drawingGroup()` usage
- Design tokens: map Figma variables to project's color/typography/spacing system; prefer `Color(.systemBackground)` over hardcoded colors

**Version targets (twostraws "Pro" series):** iOS 26+, Swift 6.2+

**Common LLM mistakes flagged:**
- Adding `.accessibilityHidden(true)` to interactive controls
- Using deprecated `NavigationView` instead of `NavigationStack`
- Hardcoding `Color.white.opacity(0.42)` instead of semantic system colors
- 260pt progress rings with mismatched stroke widths (non-proportional sizing)
- Over-engineering card backgrounds (22pt corner radius gradient cards vs native grouped content)

---

### 2. Swift Concurrency

**Signal strength:** VERY HIGH — 3 skills from 3 authors; AvdLee version has 1.3k stars (strongest concurrency signal in compilation)

**Primary sources:**
- `AvdLee/Swift-Concurrency-Agent-Skill` (v2.0.0, 1.3k stars, subdirectory `swift-concurrency/`) — most authoritative; covers Swift 6.2 features
- `twostraws/Swift-Concurrency-Agent-Skill` (subdirectory `swift-concurrency-pro/`) — iOS 26+, Swift 6.2+; focuses on LLM pitfalls
- `Dimillian/Skills` → `swift-concurrency-expert/`

**Reference files (AvdLee v2.0.0):** actors.md, async-await-basics.md, async-sequences.md, core-data.md, glossary.md, linting.md, memory-management.md, migration.md, performance.md, sendable.md, tasks.md, testing.md, threading.md

**Key topics:**
- Default Actor Isolation (Swift 6+): `@MainActor` inference rules, `nonisolated` boundaries
- `isolated deinit` (Swift 6.0+): safe cleanup of isolated state
- `global actor conformance`: correct way to mark types conforming to protocols with global actor isolation
- `nonisolated(nonsending)` (Swift 6.1+): opt-out of actor isolation for specific parameters
- `@concurrent` (Swift 6.2+): explicit concurrency annotation
- Approachable concurrency (Swift 6.2+): simplified async patterns
- `withMainSerialExecutor` for testing: deterministic test execution for actor-isolated code
- iOS 26 concurrency-safe notifications: `NotificationCenter.notifications(named:)` async sequence patterns
- `AsyncSequence`: custom sequences, `for await` loops, cancellation
- `Sendable`: when to conform, when to use `@unchecked Sendable` (with documented justification)

**Common LLM mistakes:**
- Using `DispatchQueue.main.async` in Swift 6 code instead of `await MainActor.run`
- Missing `Sendable` conformance on types shared across actor boundaries
- Creating `Task {}` inside `view.onAppear` without cancellation
- Misusing `@unchecked Sendable` to silence warnings without actual thread safety

---

### 3. Swift Testing

**Signal strength:** HIGH — 3 skills from 3 authors

**Primary sources:**
- `AvdLee/Swift-Testing-Agent-Skill` (v1.1.0, 306 stars, subdirectory `swift-testing-expert/`)
- `twostraws/Swift-Testing-Agent-Skill` (subdirectory `swift-testing-pro/`) — iOS 26+, Swift 6.2+
- `bocato/swift-testing-agent-skill` (72 stars, subdirectory `swift-testing/`)

**Reference files (AvdLee v1.1.0):** async-testing-and-waiting.md, expectations.md, fundamentals.md, migration-from-xctest.md, parallelization-and-isolation.md, parameterized-testing.md, performance-and-best-practices.md, traits-and-tags.md, xcode-workflows.md

**Reference files (bocato):** _index.md, test-organization.md, parameterized-tests.md, async-testing.md, migration-xctest.md, test-doubles.md, fixtures.md, integration-testing.md, snapshot-testing.md

**Key topics:**
- `@Test`, `#expect`, `#require`: correct usage and when to use each (`#require` for early exit on nil/failure)
- Parameterized tests: `@Test(arguments:)` for multiple inputs
- `@Suite`: grouping and scoping tests
- Traits and tags: `.tags()`, `.enabled(if:)`, `.disabled()`, `.timeLimit()`
- Async testing: `confirmation(expectedCount:)` for callbacks/delegates; `withCheckedContinuation` patterns
- Test isolation: Swift Testing runs in parallel by default; use `.serialized` trait when ordering matters
- XCTest migration: `XCTAssertEqual` → `#expect(a == b)`, `setUp/tearDown` → `init/deinit`
- Test doubles (bocato taxonomy — Martin Fowler): Dummy, Fake, Stub, Spy, SpyingStub, Mock — clarifies community confusion between spy and mock
- Fixtures: `static func fixture(...)` on model types, always inside `#if DEBUG`, close to model definition
- Exit tests (Swift Testing 6.1+): `#expect(exitsWith:)` for testing precondition failures
- Snapshot testing: UI regression patterns, `dump` snapshot testing reference

**Common LLM mistakes:**
- Using `XCTAssert` in new Swift Testing code (should use `#expect`)
- Missing `.serialized` on test suites that rely on shared mutable state
- Using `XCTestExpectation` instead of `confirmation(expectedCount:)`
- Placing test doubles outside `#if DEBUG` guards

---

### 4. SwiftData

**Signal strength:** HIGH — 2 skills from 2 authors, plus twostraws "Pro" authority

**Primary sources:**
- `twostraws/SwiftData-Agent-Skill` (subdirectory `swiftdata-pro/`) — iOS 26+, Swift 6.2+; addresses LLM pitfalls
- `vanab/SwiftData-agent-skill` (6 stars, subdirectory `swiftdata-expert-skill/`)

**Reference files (vanab):** cloudkit-sync.md, concurrency-and-actors.md, core-data-adoption.md, implementation-playbooks.md, migrations-and-history.md, model-context-and-lifecycle.md, modeling-and-schema.md, querying-and-fetching.md, relationships-and-inheritance.md, troubleshooting-and-updates.md

**Key topics:**
- `@Model`: macro requirements, stored properties, computed properties, transient properties
- `@Query`: correct usage in SwiftUI views; avoid in service layers
- `ModelContext` lifecycle: view context vs background context; never cross-context mutable model access
- `@ModelActor`: background persistence flows; identifier-based handoff pattern (`PersistentIdentifier`)
- `FetchDescriptor`: `predicate`, `sortBy`, `fetchLimit`, `fetchOffset`; stable predicate patterns
- `VersionedSchema` + staged lightweight migration (iOS 17+), custom migration plans
- CloudKit sync: `ModelConfiguration` with `cloudKitDatabase`, schema constraints (no optionals on `@Relationship`, no custom `@Attribute(.unique)` with CloudKit)
- Persistent history: processing history tokens, cleaning stale history
- Core Data coexistence: `NSPersistentCloudKitContainer` interop during migration

**Common LLM mistakes:**
- Using `@Query` in service/repository layers (only valid in SwiftUI views)
- Passing `@Model` objects across `ModelContext` boundaries without using `PersistentIdentifier`
- Missing `@ModelActor` for background work with SwiftData
- Ignoring CloudKit schema constraints in relationship design

---

### 5. Core Data

**Signal strength:** HIGH — 1 skill, AvdLee v1.0.0, 209 stars; comprehensive reference set

**Primary source:**
- `AvdLee/Core-Data-Agent-Skill` (v1.0.0, 209 stars, subdirectory `core-data-expert/`)

**Reference files:** batch-operations.md, cloudkit-integration.md, fetch-requests.md, glossary.md, migration.md, model-configuration.md, performance.md, persistent-history.md, project-audit.md, saving.md, stack-setup.md, testing.md, threading.md

**Key topics:**
- Stack setup: `NSPersistentContainer` vs `NSPersistentCloudKitContainer`, view context vs background context
- Threading: `performAndWait`, `perform(schedule:)`, main queue safety, `NSManagedObjectID` handoff
- Fetch requests: `NSFetchRequest` with `NSPredicate`, batch size, `returnsObjectsAsFaults`
- Batch operations: `NSBatchInsertRequest`, `NSBatchUpdateRequest`, `NSBatchDeleteRequest` (bypass in-memory objects — must merge changes manually)
- Composite attributes (iOS 17+): `@Attribute(.composite)` for value type storage
- Staged migration (iOS 17+), deferred migration
- Persistent history tracking: `NSPersistentHistoryChangeRequest`, token management
- CloudKit: `NSPersistentCloudKitContainer`, mirror vs share stores, `CKDatabase` scopes
- Testing: in-memory store setup, `NSInMemoryStoreType`

---

### 6. Security

**Signal strength:** HIGH — 1 skill, ivan-magda v1.0.0; 14 risk-rated topic areas; OWASP-aligned

**Primary source:**
- `ivan-magda/swift-security-skill` (v1.0.0, subdirectory `swift-security-expert/`)

**Risk-rated topic areas:**
- Keychain Fundamentals — CRITICAL
- Keychain Item Classes — HIGH
- Keychain Access Control — CRITICAL
- Biometric Authentication (LocalAuthentication framework) — CRITICAL
- Secure Enclave (key generation, `SecAccessControl`) — HIGH
- CryptoKit Symmetric Encryption (AES-GCM, ChaCha20-Poly1305) — HIGH
- CryptoKit Public Key Cryptography (P256, P384, Curve25519) — HIGH
- Credential Storage (never `UserDefaults`, never plain Keychain without access control for sensitive data) — CRITICAL
- Keychain Sharing (App Groups, `kSecAttrAccessGroup`) — MEDIUM
- Certificate Trust Evaluation (`SecTrust`, certificate pinning) — HIGH
- Migration from Legacy (deprecated `SecKeyEncrypt`, CommonCrypto) — MEDIUM
- Anti-Patterns (hardcoded secrets, `NSLog` of tokens, `.afterFirstUnlock` as default) — CRITICAL
- Testing (mocked keychain in tests, never test with production keys) — MEDIUM
- OWASP Mobile Top 10 2024 Mapping — MEDIUM

**iOS 26+ additions:** MLKEM768 (post-quantum key encapsulation), MLDSA65 (post-quantum signatures) — available in CryptoKit iOS 26+

---

### 7. Architecture

**Signal strength:** MEDIUM — 1 skill, efremidze v0.4.3; 8 patterns with playbooks

**Primary source:**
- `efremidze/swift-architecture-skill` (v0.4.3, subdirectory `swift-architecture-skill/`)

**Reference files:** selection-guide.md + one playbook per pattern (mvp.md, mvvm.md, mvi.md, tca.md, clean-architecture.md, viper.md, coordinator.md, reactive.md)

**Supported patterns and when to use:**
- MVP: simple screens, testable presenters, UIKit-heavy codebases
- MVVM: SwiftUI-first, `@Observable`/`@ObservationIgnored`, `Combine`
- MVI: unidirectional data flow, complex state machines
- TCA (The Composable Architecture): feature composition, navigation, effects, dependencies
- Clean Architecture: large teams, strict layer separation, use cases
- VIPER: module isolation, legacy codebases, strict interface contracts
- Coordinator: navigation decoupling, deep linking
- Reactive: `Combine`/`RxSwift` pipeline-centric flows

**Key signal:** Critiques LLM default behavior of applying MVVM to every project regardless of size, team, or framework choice. Selection guide provides decision criteria.

---

### 8. Swift Language / API Design

**Signal strength:** MEDIUM — 1 skill, Erikote04; anchored in official Swift API Design Guidelines

**Primary source:**
- `Erikote04/Swift-API-Design-Guidelines` (subdirectory `swift-api-design-guidelines-skill/`)

**Reference files:** argument-labels.md, fundamentals.md, general-conventions.md, parameters.md, promote-clear-usage.md, special-instructions.md, strive-for-fluent-usage.md, use-terminology-well.md

**Key topics:**
- Fluent usage: method calls read as grammatical phrases (`x.insert(y, at: z)`)
- Argument labels: omit when obvious from context, include when they clarify meaning
- Naming: clarity at the point of use > brevity; avoid abbreviations
- Parameters: default parameters over overloads; use labeled trailing closures
- Protocols: noun names for capabilities (`Collection`), adjective suffixes for properties (`Comparable`)
- Terminology: use established terms correctly; prefer plainer alternatives when terminology is not established

---

### 9. Accessibility

**Signal strength:** HIGH — 3 skills from 3 authors covering UIKit, SwiftUI, and AppKit

**Primary sources:**
- `dadederk/iOS-Accessibility-Agent-Skill` (128 stars, subdirectory `ios-accessibility/`) — most comprehensive reference set; UIKit + SwiftUI
- `PasqualeVittoriosi/swift-accessibility-skill` (v0.2.0, 53 stars, subdirectory `swift-accessibility-skill/`) — covers all 9 App Store Accessibility Nutrition Labels
- `rgmez/apple-accessibility-skills` (v1.1.1, 16 stars) — 3 separate auditor skills: `swiftui-accessibility-auditor/`, `uikit-accessibility-auditor/`, `appkit-accessibility-auditor/`; severity-rated findings (P0/P1/P2)

**Reference files (dadederk — 17 files):** voiceover.md, voiceover-uikit.md, voiceover-swiftui.md, dynamic-type.md, dynamic-type-uikit.md, dynamic-type-swiftui.md, voice-control.md, switch-control.md, full-keyboard-access.md, good-practices.md, concepts-and-culture.md, testing-manual.md, testing-automated.md, playbook.md, glossary.md, resources.md (note: media-accessibility.md in PasqualeVittoriosi)

**Reference files (PasqualeVittoriosi):** voiceover-swiftui.md, voiceover-uikit.md, voice-control.md, dynamic-type.md, display-settings.md, semantic-structure.md, media-accessibility.md, motor-input.md, nutrition-labels.md, assistive-access.md, platform-specifics.md, testing-auditing.md, wcag-mapping.md

**App Store Accessibility Nutrition Labels (all 9):**
- VoiceOver, Voice Control, Larger Text, Dark Interface, Differentiate Without Color, Sufficient Contrast, Reduced Motion, Captions, Audio Descriptions

**Key topics:**
- VoiceOver: `accessibilityLabel`, `accessibilityValue`, `accessibilityHint`, `accessibilityTraits`, custom actions, grouping (`accessibilityElement(children:)`)
- 44×44pt minimum touch target (Apple HIG requirement)
- Dynamic Type: `scaledFont`, `.dynamicTypeSize`, `@ScaledMetric`, never hardcode font sizes
- Color contrast: minimum 4.5:1 (WCAG AA), 3:1 for large text
- Multi-modal information: never convey information by color alone
- Switch Control + Full Keyboard Access: focus order, escape key support
- Assistive Access (iOS 17+): simplified UI mode
- AppKit: `NSAccessibility` protocol, `accessibilityRole`, `NSAccessibilityElement`
- Testing: `XCUIAccessibilityAudit` (Xcode 15+), manual VoiceOver testing checklist, Accessibility Inspector

**rgmez audit principles:** P0 (blocking/broken), P1 (significant barrier), P2 (improvement); minimal localized fixes; no architecture rewrites; always include manual verification steps

---

### 10. App Store / DevOps

**Signal strength:** HIGH — 2 skills; rudrankriyam is the dominant source (629 stars, 21 sub-skills)

**Primary sources:**
- `rudrankriyam/app-store-connect-cli-skills` (629 stars, subdirectory `skills/`) — 21 skills for the `asc` CLI tool (asccli.sh)
- `timbroddin/app-store-aso-skill` (v1.0.0, 17 stars) — ASO optimization; 47KB knowledge base; June 2025 screenshot caption OCR update

**rudrankriyam `asc` CLI skills (21 skills):**
- `asc-cli-usage`: canonical verbs, flags, pagination, JSON-first output, auth
- `asc-workflow`: `.asc/workflow.json`, hooks (`before_all`, `after_all`, `error`), conditionals, cycle validation
- `asc-app-create-ui`: browser automation for app record creation
- `asc-xcode-build`: archive, export, `ExportOptions.plist`, encryption compliance
- `asc-shots-pipeline`: screenshot automation (xcodebuild/simctl + AXe + capture → frame → upload)
- `asc-release-flow`: readiness-first submission (`asc release stage`, `asc submit preflight`)
- `asc-signing-setup`: bundle IDs, capabilities, certificates, provisioning profiles
- `asc-id-resolver`: deterministic ID resolution for apps/builds/versions/groups
- `asc-metadata-sync`: metadata pull/push, character limit validation, legacy format migration
- `asc-localize-metadata`: LLM translation with locale-aware keywords, strict character limits
- `asc-aso-audit`: offline ASO audit of `./metadata` + Astro MCP keyword-gap analysis
- `asc-whats-new-writer`: polished release notes from git log or bullet points, localized
- `asc-submission-health`: preflight checks, digital goods readiness, "version not in valid state" troubleshooting
- `asc-testflight-orchestration`: beta groups, testers, build distribution, What to Test notes
- `asc-build-lifecycle`: build processing, latest build resolution, cleanup/retention
- `asc-ppp-pricing`: territory-specific pricing with PPP strategy
- `asc-subscription-localization`: bulk-localize subscription/IAP display names
- `asc-revenuecat-catalog-sync`: audit-first ASC ↔ RevenueCat reconciliation
- `asc-notarization`: macOS Developer ID signing, notarization, stapling
- `asc-crash-triage`: TestFlight crashes grouped by signature, beta feedback, performance diagnostics
- `asc-wall-submit`: Wall of Apps submission

**timbroddin ASO key knowledge:**
- Metadata indexing hierarchy: Title (highest) > Subtitle > Screenshot Captions (NEW June 2025 OCR) > Keywords
- Character limits: Name 30, Subtitle 30, Keywords 100, Promo Text 170, Description 4000
- No keyword duplication across title/subtitle/keywords field
- Screenshot captions now OCR-indexed (June 2025 algorithm update) — requires high-contrast text
- Rating target: 4.5+ stars for optimal visibility
- Update frequency: every 2-4 weeks minimum
- Krankie CLI for local keyword ranking tracking (SQLite, agent-first)

---

### 11. iOS Simulator

**Signal strength:** HIGH — 1 skill, 670 stars; production-grade; battle-tested with eval benchmarks

**Primary source:**
- `conorluddy/ios-simulator-skill` (v1.3.1, 670 stars, subdirectory `ios-simulator-skill/`) — 21 Python scripts

**21 script categories:**
- Build & Development: `build_and_test.py`, `log_monitor.py`
- Navigation & Interaction: `screen_mapper.py`, `navigator.py` (semantic by text/type/ID, not pixel coordinates), `gesture.py`, `keyboard.py`, `app_launcher.py`
- Testing & Analysis: `accessibility_audit.py` (WCAG compliance), `visual_diff.py` (screenshot comparison), `test_recorder.py`, `app_state_capture.py`, `sim_health_check.sh`
- Device Lifecycle: `simctl_boot.py`, `simctl_shutdown.py`, `simctl_create.py`, `simctl_delete.py`, `simctl_erase.py`
- Permissions: `clipboard.py`, `status_bar.py`, `push_notification.py`, `privacy_manager.py`

**Design principles:**
- Semantic navigation (accessibility APIs) — survives UI redesigns, works across device sizes
- Token efficiency: 96% reduction vs raw tools (5 lines default, `--verbose` for details, `--json` for CI/CD)
- Auto-UDID detection — no per-command device specification needed
- Eval-validated: 100% success (3/3) with skill vs 46% without

**Prerequisites:** macOS 12+, Xcode CLT, Python 3; IDB optional for interactive features

---

### 12. UI Design / Design Handoff

**Signal strength:** LOW-MEDIUM — 2 skills, newer/lower stars, but fills a genuine gap

**Primary sources:**
- `arjitj2/swiftui-design-principles` (5 stars, `SKILL.md` at repo root — atypical)
- `daetojemax/figma-to-swiftui-skill` (8 stars, subdirectory `figma-to-swiftui/`)

**arjitj2 design principles (10 rules):**
1. Spacing system: base-4/8 grid (avoid arbitrary values like 26, 34, 36pt)
2. Typography hierarchy: weight-based (avoid 7+ font sizes with no system)
3. Semantic system colors (avoid hardcoded `Color.white.opacity(0.42)`)
4. Proportional component sizing (avoid 260pt progress rings)
5. Native grouped content (avoid over-engineered gradient cards)
6. `NavigationStack` usage (avoid bare `ZStack` layouts)
7. WidgetKit native components — use `Gauge` instead of manual circle drawing
8. Interactive elements — visible Toggle labels, sufficient color contrast
9. Shared data models between app and widget
10. Pre-ship checklist

**daetojemax Figma-to-SwiftUI (8-step workflow):**
1. Parse Figma URL → 2. Fetch design context via Figma MCP → 3. Capture screenshot → 4. Fetch design tokens → 5. Download assets → 6. Implement in SwiftUI → 7. Validate (on request) → 8. Register Code Connect mappings

**Key principle:** Figma MCP returns React + Tailwind — treat as spec, never port web code. Use project's existing color/typography/spacing system. Prefer SF Symbols over custom icons. Skip system-provided elements (keyboard, status bar, etc.).

**Skill structure (daetojemax):** layout-translation.md, responsive-layout.md, design-token-mapping.md, component-variants.md, asset-handling.md, figma-mcp-setup.md

---

### 13. UX Writing

**Signal strength:** LOW-MEDIUM — 1 skill, thoughtfully scoped; sources anchored in Apple WWDC + HIG

**Primary source:**
- `andrewgleave/skills` → `writing-for-interfaces/` (subdirectory `writing-for-interfaces/`)

**Reference files:** `references/patterns.md` — detailed guidance for common interface patterns

**Workflow (6 steps):**
1. Establish voice (search CLAUDE.md/AGENTS.md/style guides first; if missing, interview user)
2. Evaluate request type (new copy, review, rewrite, or terminology work)
3. Apply voice and principles
4. Consult patterns reference for situation-specific guidance
5. Apply changes (show original → rewrite with rationale; prioritize blocking issues over polish)
6. Update terminology reference (flag drift, suggest word list entries)

**WWDC sources referenced:** WWDC 2019 (Accessibility Labels), 2022 (Writing for Interfaces), 2024 (Personality through UX Writing), 2025 (Big Impact with Small Writing Changes), Apple Style Guide, HIG Writing/Alerts/Accessibility

---

## Cross-Cutting Observations

**Version targeting:** The twostraws "Pro" series explicitly targets iOS 26+, Swift 6.2+. Use these as the authoritative upper-bound source. AvdLee concurrency covers Swift 6.0–6.2 features progressively.

**LLM-specific corrections:** Multiple skills (twostraws SwiftUI, Architecture, Accessibility) explicitly document mistakes that LLMs make. These are high-value inputs — directly inform what constraints to encode in generated agents.

**Multi-skill repos:** Dimillian/Skills contains 9+ sub-skills in one repo. When reading, use the subdirectory name as the skill boundary (e.g., `swiftui-liquid-glass/`, not the parent repo).

**App Store tooling dependency:** rudrankriyam's skills depend on the `asc` CLI (asccli.sh). Do not generate App Store automation agents based on these skills unless the project uses or plans to adopt `asc`.

**Architecture neutrality:** Architecture skill explicitly warns against MVVM defaults. Generated agents should not prescribe architecture unless the project has established one.

---

## Unavailable Skills

None. All 17 repo pages were successfully fetched. All skills are MIT-licensed.

Note: `vanab/SwiftData-agent-skill` and `arjitj2/swiftui-design-principles` have very low star counts (6 and 5 respectively). Weight their content lower when it conflicts with higher-signal sources (twostraws, AvdLee).

---

## Generation Implications

| Domain | Signal | Use in generation |
|---|---|---|
| SwiftUI | VERY HIGH | Always include for any SwiftUI project; encode LLM mistake corrections as explicit constraints |
| Swift Concurrency | VERY HIGH | Include for any async code; use AvdLee v2.0.0 as primary source; verify Swift version target |
| Swift Testing | HIGH | Include for test agents; bocato adds test double taxonomy missing from twostraws/AvdLee |
| SwiftData | HIGH | Include for any persistence layer using SwiftData |
| Core Data | HIGH | Include when project uses Core Data; covers iOS 17+ staged migration |
| Security | HIGH | Include when project handles credentials, biometry, or sensitive data; OWASP-aligned |
| Architecture | MEDIUM | Include only when project architecture is unclear or needs selection guidance |
| API Design | MEDIUM | Include for library/SDK projects or when code review is a workflow goal |
| Accessibility | HIGH | Include for any consumer-facing app; use P0/P1/P2 severity framework from rgmez |
| App Store / DevOps | HIGH | Include for CI/CD and release agents; verify `asc` CLI dependency before use |
| iOS Simulator | HIGH | Include for any agent that runs builds or interacts with simulator |
| UI Design | LOW-MEDIUM | Include for greenfield projects; skip if design system is already established |
| UX Writing | LOW-MEDIUM | Include when copy quality is a stated goal; requires voice definition step |
