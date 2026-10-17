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

1. **Determine scope**: platform(s), app category, target territories/storefronts, user age group, business model, current vs proposed behavior, whether submission is an update or new app. Derive from repo and provided context first. If unknown, mark relevant conclusions conditional.
2. **Inspect evidence**: source, Xcode project, entitlements, Info.plist/localized purpose strings, privacy manifests, Package.swift/Podfile/SDK list, StoreKit code/products, networking/data flows, onboarding/auth/account deletion, permissions, background modes, HealthKit integrations, metadata/screenshots, reviewer instructions and release notes. Do not claim to have checked unprovided artifacts.
3. **Map applicable sections** using `references/review-matrix.md`; read all five guideline areas, not just the obvious one. Look for interactions (e.g., HealthKit + advertising; subscriptions + web checkout; social login + account deletion).
4. **Verify policy** against the live Apple URL and exact numbered sections. Review region-specific exceptions separately; do not generalize US-storefront provisions worldwide.
5. **Report findings** with severity, guideline number, precise evidence (file/line/UI path), concrete user-visible impact, remediation, and validation test. Categorize as `Confirmed violation`, `Likely rejection risk`, `Requires manual verification`, or `Pass (verified)`.
6. **Fix when explicitly asked and authorized**: make minimal changes, preserve functionality and architecture, add or update tests, and explain remaining manual App Store Connect steps. Never change purchase or privacy behavior silently.
7. **Prepare release gate**: audit binary and metadata separately; draft precise App Review notes; request demo credentials only through secure channels (never echo credentials in final reports); identify backend dependencies and regional availability.

## Priority checks

### Safety — section 1

- 1.1 prohibited/objectionable material, deceptive functionality.
- 1.2 user-generated content and AI creation: content filtering, reporting, timely response, blocking, visible support contact; creator-content and age restrictions where applicable.
- 1.3 Kids Category: parental gates, restrictions on advertising/analytics, children’s privacy.
- 1.4 health/medical accuracy and potentially harmful advice: validate claims and methods, ensure appropriate physician guidance, distinguish measurement from estimates. Drug-dose calculators have stringent restrictions.
- 1.5 support contact and 1.6 security safeguards.

### Performance — section 2

- 2.1 finished/tested build, reviewable purchases, functional URLs, live backend, demo credentials or approved demo mode; 2.2 no beta-only App Store listing.
- 2.3 honest screenshots, text, pricing, age rating, permissions, feature descriptions, reviewer notes, What's New, correct app name/keyword usage. App name maximum 30 characters.
- 2.4 compatibility, battery/device safety.
- 2.5 public APIs, sandbox and executable-code restrictions, authorized background modes, IPv6, WebKit rules, recording consent, widgets/extensions, honest feature disclosure. Inspect App Intents/Siri for relevant task fulfillment.

### Business — section 3

- 3.1.1 digital feature/content unlocking normally requires IAP; verify exemptions and **storefront-specific** external purchase rules, especially the US exception vs other regions and entitlement requirements.
- 3.1.2 auto-renewable subscriptions: ongoing value, minimum period, clear offer terms, access on supported devices, appropriate upgrades/downgrades and restoration. Beware misleading paywalls and loss of prior paid entitlements.
- 3.1.3 classify reader apps, multiplatform products, real-time person-to-person services, physical goods/services and other exceptions carefully; don't assume exemption.
- 3.1.5 cryptocurrency requirements as relevant.
- 3.2 financial services, money management, investing, financial licenses and legal-entity requirements; fundraising, deceptive rankings, gambling/loan restrictions when applicable.

### Design — section 4

- 4.1 copied branding/UI, 4.2 substantive native utility, 4.3 spam/near-duplicate binaries, 4.4 extensions, 4.5 Apple services/branding, 4.7 in-app software/mini apps/streaming games/chatbots if applicable.
- 4.8 if social login establishes the primary account, verify an equivalent privacy-preserving login option **or an explicit exception**; do not state that Sign in with Apple is invariably required by the rule's exact wording.
- 4.9 Apple Pay recurring terms; 4.10 restrictions on monetizing built-in OS/device capabilities.

### Legal/privacy — section 5

- 5.1.1 in-app and App Store Connect privacy policy links, accurate purposes, informed consent and withdrawal, data minimization, account deletion from within the app when account creation exists, third-party SDK disclosures.
- 5.1.2 third-party sharing including third-party AI, explicit permission, ATT when tracking, no repurposing without necessary consent, sensitive-data restrictions.
- 5.1.3 HealthKit data accuracy, no personal health information in iCloud, restrictions on health-data advertising and data mining, research consents if applicable.
- 5.1.4 children’s data, 5.1.5 location consent, 5.2 ownership/licensing and third-party terms, 5.3 gambling/lotteries, 5.4 VPN, 5.5 MDM, 5.6 honest reviews/developer conduct. Evaluate only relevant subcategories but explicitly mark excluded categories as not applicable.

## Repository searches (examples, not sufficient proof)

Use appropriate available search tools (`rg` if supported):

```bash
rg -n 'StoreKit|Product\.purchase|Transaction\.currentEntitlements|restorePurchases|SKPayment|subscription' .
rg -n 'HKHealthStore|HealthKit|HKSample|NSHealth|Clinical|oxygenSaturation' .
rg -n 'ASAuthorization|SignInWithApple|GoogleSignIn|FBSDKLogin|deleteAccount' .
rg -n 'ATTrackingManager|AppTrackingTransparency|AdSupport|FirebaseAnalytics|Mixpanel|Amplitude' .
rg -n 'NSCameraUsageDescription|NSMicrophoneUsageDescription|NSLocation|NSPhoto|UIBackgroundModes' .
rg -n 'PrivacyInfo.xcprivacy|NSPrivacyCollectedDataTypes|NSPrivacyAccessedAPITypes' .
```

Search results are leads, not verdicts. Inspect surrounding implementation, UI paths, target membership and live configuration.

## Required report format

```
# App Store Review Readiness — <App> (<version/build>)
Policy: Apple App Review Guidelines (verified <date>; source last updated <date or unknown>)
Scope: <targets, storefronts, build/artifacts reviewed, exclusions>
Summary: <confirmed violations / likely risks / manual checks / verified passes>

## Findings (highest severity first)
- [BLOCKER/HIGH/MEDIUM/LOW] [Confirmed/Likely/Manual] Guideline <number> — <short title>
  Evidence: <repo file:line, screenshot, metadata, observed behavior, or "not supplied">
  Why it matters: <policy application; cite official subsection URL>
  Fix: <specific action>
  Validate: <reproduction or acceptance test>

## App Store Connect and human-only checks
<requirements not verifiable in code>

## Proposed App Review notes
<accurate explanation of non-obvious features, login/demo, IAP, permissions, regulatory documentation>

## Release gate
<Ready / Not ready / Undetermined with reasons>
```

Never invent source locations, regulatory approvals, entitlements, account configuration, or reviewer decisions. Absence of evidence is `Manual` or `Unknown`, not `Pass` or `Violation` unless the missing artifact itself establishes a failure. A clean static audit cannot guarantee approval.

## Companion reference

Read `references/review-matrix.md` for a concise rule-to-evidence mapping, conditional trigger list, and release checklist. The full Apple guideline, not this summary, controls interpretation.
