---
name: apple-app-review-compliance
description: Audit Apple-platform applications, proposed features, code changes, App Store metadata, privacy practices, and submission readiness against Apple's current App Review Guidelines. Use during feature planning, PR review, pre-release QA, App Store rejection triage, or whenever StoreKit, HealthKit, permissions, authentication, user-generated content, or App Review policy matters.
---

# Apple App Review Compliance

Act as a careful Apple App Review readiness auditor for iOS, iPadOS, watchOS, macOS, tvOS and visionOS. Find actionable risks, cite exact guideline subsections, propose minimal compliant changes, and distinguish proven defects from unverified assumptions.

## Authoritative source and freshness

- Primary source: https://developer.apple.com/app-store/review/guidelines/
- The source showed **Last Updated: October 16, 2026** when this skill was authored (October 17, 2026). This is a snapshot, NOT permanent policy authority.
- At each substantive review, retrieve the live guidelines if internet access is available. Record the page's last-updated date. If unavailable, disclose that live verification was not possible; never assert that exceptions, entitlements, regional payment rules or deadlines remain current.
- For complex platform rules, inspect links to relevant Apple Developer documentation, App Store Connect help, and current entitlement requirements. Official policy takes precedence over this skill.
- Treat guidance as review-readiness assessment, not guaranteed approval or legal advice. Do not suggest hiding functionality from reviewers.

## When to use

Trigger on: App Store submission/review, rejection response, feature or monetization design, Swift/SwiftUI PR review, privacy policy, App Privacy labels, third-party SDK auditing, subscriptions and external payments, age rating, app screenshots and metadata, HealthKit, Apple Watch, digital content, UGC, AI content, or account sign-in/deletion.

## Workflow

1. **Infer the review mode** from the request: feature planning (proposed behavior and acceptance criteria), PR review (changed behavior and affected integrations), submission audit (release build and submission evidence), or rejection triage (Apple’s message, affected build, reproduction and response). State the mode. Keep a focused review concise; provide full reviewer notes and submission conclusions for submission audits or when relevant to triage.
2. **Establish scope and provenance**: record commit and dirty-worktree state, or base/head and diff range; targets, version/build, scheme/configuration, OS/device and build availability. Record unknowns. Derive category, age group, business model, storefronts, and new app versus update from available evidence. Separate current behavior from plans.
3. **Inspect evidence**: discover files by name before content searches below. Label evidence as source inspection, supplied artifact (with version/date), observed runtime behavior (environment and result), configuration evidence (local or live), or assumption. Source code does not establish runtime behavior or live App Store Connect settings.
4. **Screen all five areas**—Safety, Performance, Business, Design, Legal—for applicability using [references/review-matrix.md](references/review-matrix.md), then investigate relevant areas to the depth needed for the request. Record applicability reasons and unknown triggers. Put user exclusions and unreviewed areas under **Outside scope / Not reviewed**, even if screening suggests relevance.
5. **Verify policy** against retrieved official sections, recording successful retrieval separately from failed attempts and packaged snapshot knowledge. Check platform/storefront exceptions separately. Unsupported assumptions stay conditional.
6. **Report** using the statuses and evidence rules below. Separate completed validation from proposed steps. For authorized fixes, use the user’s existing authorization; make minimal changes and explain purchase/privacy behavior changes. Verify remediation meaningfully: add/update tests when they establish changed behavior, run relevant existing checks, and record failures or unavailable checks. Documentation-only changes need appropriate content/link/install checks rather than artificial behavioral tests.
7. **Conclude within scope**. For submission audits, assess binary and metadata, use the release checklist, and draft reviewer notes covering non-obvious features, access/demo setup, IAP, permissions, backend dependencies, regional availability and required documents. Keep unavailable details marked pending; never print credentials. For triage, tie proposed response and remediation to Apple’s stated concern. A focused PR review does not establish whole-app submission readiness.

## Priority checks

### Safety — section 1

- 1.1 prohibited/objectionable material, deceptive functionality.
- 1.2 content creation, generation, upload or contribution, including UGC/social, chat and AI creation apps (not limited to shared outputs): content filtering, reporting, timely response, blocking, visible support contact; creator-content and age restrictions where applicable.
- 1.3 Kids Category: parental gates, restrictions on advertising/analytics, children’s privacy.
- 1.4 health/medical accuracy and potentially harmful advice: validate claims and methods, ensure appropriate physician guidance, distinguish measurement from estimates. Drug-dose calculators have stringent restrictions.
- 1.5 support contact and 1.6 security safeguards.

### Performance — section 2

- 2.1 finished/tested build, reviewable purchases, functional URLs, live backend, demo credentials or approved demo mode; 2.2 no beta-only App Store listing.
- 2.3 honest screenshots, text, pricing, age rating, permissions, feature descriptions, reviewer notes, What's New, correct app name/keyword usage. App name maximum 30 characters.
- [2.3.14](https://developer.apple.com/app-store/review/guidelines/#accurate-metadata) creative assets: inspect relevance, audience suitability and misleading claims/pricing.
- [2.5.19](https://developer.apple.com/app-store/review/guidelines/#software-requirements) hidden, dormant or undocumented features: inspect remote flags and reviewer access.
- 2.4 compatibility, battery/device safety.
- 2.5 public APIs, sandbox and executable-code restrictions, authorized background modes, IPv6, WebKit rules, recording consent, widgets/extensions, honest feature disclosure. Inspect App Intents/Siri for relevant task fulfillment.

### Business — section 3

- 3.1.1 digital feature/content unlocking normally requires IAP; verify exemptions and **storefront-specific** external purchase rules, especially the US exception vs other regions and entitlement requirements.
- 3.1.2 auto-renewable subscriptions: ongoing value, minimum period, clear offer terms, access on supported devices, appropriate upgrades/downgrades and restoration. Beware misleading paywalls and loss of prior paid entitlements.
- 3.1.3 classify reader apps, multiplatform products, real-time person-to-person services, physical goods/services and other exceptions carefully; don't assume exemption.
- 3.1.5 cryptocurrency requirements as relevant.
- 3.2.1(viii) financial trading, investing or money-management services: verify provider and territorial licensing. Assess 5.1.1(ix) separately for legal-entity submission in regulated or sensitive-information services. A ledger alone does not establish regulated financial services; inspect actual behavior. Also assess fundraising, rankings, gambling/loans as relevant.

### Design — section 4

- For new apps and updates: 4.1 copied branding/UI, 4.2 substantive native utility, 4.3 spam/near-duplicate binaries, 4.4 extensions, 4.5 Apple services/branding, 4.7 in-app software/mini apps/streaming games/chatbots if applicable.
- 4.8 if third-party or social login sets up or authenticates the primary account, verify an equivalent privacy-preserving login option **or an explicit exception**; do not state that Sign in with Apple is invariably required by the rule's exact wording.
- 4.9 Apple Pay recurring terms; 4.10 restrictions on monetizing built-in OS/device capabilities.

### Legal/privacy — section 5

- 5.1.1 in-app and App Store Connect privacy policy links, accurate purposes, informed consent and withdrawal, data minimization, account deletion from within the app when account creation exists, third-party SDK disclosures.
- 5.1.2 third-party sharing including third-party AI, explicit permission, ATT when tracking, no repurposing without necessary consent, sensitive-data restrictions.
- 5.1.3 health, fitness and medical data, including data outside HealthKit: data accuracy, no personal health information in iCloud, restrictions on health-data advertising and data mining, research consents if applicable.
- 5.1.4 children’s data, 5.1.5 location consent, 5.2 ownership/licensing and third-party terms, 5.3 gambling/lotteries, 5.4 VPN, 5.5 MDM, 5.6 honest reviews/developer conduct. Investigate relevant subcategories; support each Not applicable decision with evidence. Exclusions belong under Outside scope / Not reviewed.

## Repository searches (examples, not sufficient proof)

First inventory filenames from the app root (`rg --files --hidden`, adding `--no-ignore` for relevant ignored/generated locations). For example:

```bash
rg --files --hidden -g '*.xcprivacy' -g '*.plist' -g '*.entitlements' -g '*.pbxproj' -g '*.xcworkspacedata' -g '*.xcconfig' -g '*.xcscheme' -g '*.storekit' -g 'Package.swift' -g 'Package.resolved' -g 'Podfile*' -g 'Cartfile*' -g 'package.json' -g '*lock*' -g 'project.yml' -g 'Project.swift'
```

Inspect discovered files and relevant dependencies. Resolve generated Info.plist/build settings, configuration substitutions, target membership, resource-copy phases and the built archive when available. Record missing access to generated artifacts. A filename or content search with no hits is not proof of absence.

Then search contents using appropriate available tools (`rg` if supported):

```bash
rg -n 'StoreKit|Product\.purchase|Transaction\.currentEntitlements|restorePurchases|SKPayment|subscription' .
rg -n 'HKHealthStore|HealthKit|HKSample|NSHealth|Clinical|oxygenSaturation' .
rg -n 'ASAuthorization|SignInWithApple|GoogleSignIn|FBSDKLogin|deleteAccount' .
rg -n 'ATTrackingManager|AppTrackingTransparency|AdSupport|FirebaseAnalytics|Mixpanel|Amplitude' .
rg -n 'NSCameraUsageDescription|NSMicrophoneUsageDescription|NSLocation|NSPhoto|UIBackgroundModes' .
rg -n 'PrivacyInfo.xcprivacy|NSPrivacyCollectedDataTypes|NSPrivacyAccessedAPITypes' .
```

Search results are leads, not verdicts. Inspect surrounding implementation, UI paths, target membership and live configuration.

## Status and verdict rules

Use these exact status names in findings, coverage and checklists; severity is separate:

| Status | Meaning |
|---|---|
| Confirmed violation | Evidence establishes both rule applicability and a concrete breach; cite the policy basis and its freshness limitations. |
| Likely rejection risk | Evidence supports a probable policy problem, but interpretation or an essential fact remains uncertain. State that uncertainty. |
| Requires manual verification | An applicable or potentially applicable check lacks sufficient evidence; specify what would resolve it. |
| Pass (verified) | A stated check was performed and evidence supports satisfaction in the stated build/configuration and scope. |
| Not applicable | Evidence establishes that the rule’s trigger does not apply; record the reason. |

**Outside scope / Not reviewed** is a scope ledger, not a finding status. Missing evidence is not a pass, a violation, or proof of inapplicability unless the missing item itself demonstrably breaches an applicable requirement.

Release verdicts:

- **Not ready**: a known material blocker remains, including a material likely risk that must be resolved before release. This takes precedence over unknowns; list both.
- **Undetermined**: no known material blocker, but submission-critical evidence or current policy applicability is unresolved. State the missing evidence.
- **Ready (within stated verified scope)**: all applicable critical checks in that scope have evidence and no material blockers or critical unknowns remain. Name the scope and limitations; this is not guaranteed Apple approval. A PR may be ready within its reviewed changes while whole-app submission readiness remains unassessed.

## Report format (scale depth to mode)

```text
App Review — <app, version/build or unknown>
Mode: <feature planning / PR review / submission audit / rejection triage>
Revision: <commit + local changes, or base...head; reviewed artifacts>
Build/configuration: <targets, scheme, OS/device, artifact identity; unavailable details>
Scope: <behaviors, platforms, storefronts>
Policy basis: <live retrieval succeeded / partial retrieval / snapshot-only>
Retrieval: <successful retrieval date per source, or none; failed attempt date/reason>
Apple observed update date: <date displayed on retrieved page or unknown>
Snapshot basis (if used): <packaged date and recorded Apple date; not live verification>
Sources actually consulted: <URLs and sections read; local references separately>

Findings (highest severity first):
- <severity> | <exact status> | <guideline number and official link>
  Evidence: <type, file:line/artifact/UI path; assumptions separately>
  Impact and policy applicability: <reason, storefront/exception conditions>
  Remediation: <specific action>
  Suggested validation: <acceptance criteria; not claimed as performed>

Coverage:
| Area/check | Applicability and reason | Status | Evidence and limits |
<compact rows for all five area screens, verified checks and inapplicability decisions>

Outside scope / Not reviewed: <excluded or unexamined areas and reason>
Validation performed: <actual command/action, environment, result and evidence; none if none>
Suggested validation: <remaining tests and artifacts needed>
App Store Connect / human checks: <unknowns, owner or next action>
Proposed App Review notes (when relevant): <accurate access/setup and feature notes; pending details>
Release verdict (when relevant): <defined verdict, scope, blockers and critical unknowns>
```

For feature planning, state that implementation/runtime verification remains pending. For a focused PR, give a scoped conclusion and explicitly leave whole-submission readiness unassessed. In snapshot-only operation, do not put inaccessible URLs in “Sources actually consulted” or describe an attempted retrieval as verification.

## Companion reference

Read [references/review-matrix.md](references/review-matrix.md) for applicability, account-deletion detail and the submission checklist. Official policy controls interpretation.
