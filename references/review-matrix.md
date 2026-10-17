# App Review evidence matrix

Source: https://developer.apple.com/app-store/review/guidelines/ (Apple page last updated October 16, 2026 as observed October 17, 2026). Refresh before relying on changing rules.

| Rule | Trigger | Evidence to inspect | Typical validation |
|---|---|---|---|
| 1.2 | UGC, public profiles, AI-generated shared content | reporting/block/mute/moderation flows, contact | report/block abusive content in production-like environment |
| 1.3 / 5.1.4 | child-directed experience | category, ads SDKs, consent, parental gates | age-appropriate walkthrough |
| 1.4 | health claims, disease decisions, sensor readings | wording, calculation code, clinical basis, disclaimers | compare accuracy claims to sources; test harmful advice paths |
| 2.1 | every submission | startup, links, reviewer demo, backend and IAP visibility | fresh-install E2E on supported OS |
| 2.3 | every submission | listing metadata, screenshots, What's New, age rating | match live UI to every public claim |
| 2.5 | platform APIs, background work, recording, SDKs | entitlements, Info.plist, frameworks, permissions | check purpose text and background behavior on device |
| 3.1.1 | premium digital features and purchases | StoreKit, purchase links, regions, entitlement | sandbox purchase, restore, offline/relaunch behavior |
| 3.1.2 | auto-renew subscriptions | paywall, product config, terms, upgrade paths | billing disclosures, renewal/cancel flow, entitlement state |
| 3.2.1(viii) / 5.1.1(ix) | money management/investing/regulated sector | actual regulated services, entity, licensing | legal-entity/regulatory manual validation |
| 4.1–4.3 | every new app | differentiation, UX, branding, bundle variants | verify original meaningful app functionality |
| 4.8 | third-party primary-account social login | auth flow, alternatives, applicable exceptions | equivalent sign-in or supported exception |
| 5.1.1 | all apps | privacy policy URL, SDKs, purpose strings, deletion | test withdrawal/deletion and policy availability |
| 5.1.2 | tracking, analytics, third-party AI | SDK/network flow, permission/ATT, AI disclosure | verify purpose-specific consent before sharing |
| 5.1.3 | HealthKit or medical research | read/write types, cloud sync, sharing recipients | no fabricated HealthKit data; sensitive data handled correctly |
| 5.2 | third-party brands/APIs/assets | license files, API terms, graphics, music | verify rights and trademark permissions |
| 5.6 | ratings/reviews/growth | review prompts, incentivization | system-supported rating prompt, no manipulation |

## Specific pitfalls

- Distinguish **health data in HealthKit** from **health data stored elsewhere**; Apple specifically prohibits storing personal health information in iCloud under 5.1.3(ii).
- Distinguish **financial record-keeping** from **licensed financial services**. Avoid assuming a personal ledger app is a bank or that every finance app must be operated by a bank; investigate actual service and policy applicability.
- Third-party AI processing of personal data needs explicit disclosure and permission under 5.1.2(i).
- Do not conflate all external payment links: region, type of app, entitlement, and applicable policies matter; verify on the day of review.
- A watchOS complication, widget, App Clip, extension or App Intent must match main-app functionality and its specific policies.
- App Privacy label accuracy and privacy-manifest implementation require inspection of dependencies **and** data practices, not keyword scans alone.
- Apple guideline 4.8 describes privacy properties of an equivalent sign-in service and exceptions; assess exact conditions rather than blindly requiring one brand.

## Release checklist

- [ ] Test cold launch, upgrade, background/foreground, offline, permissions denied, iPad/watch as applicable
- [ ] Remove placeholders/debug endpoints; ensure all links and review backend work
- [ ] Confirm App Review can access gated features, configured IAPs, and test data
- [ ] Confirm app and IAP descriptions, screenshots, age rating, What's New and reviewer notes match build
- [ ] Inspect paywall pricing, subscription terms, receipts, restore, family/device availability as applicable
- [ ] Review collection/sharing/SDK use and ensure privacy policy and App Privacy labels are consistent
- [ ] Exercise account deletion, logout and permission withdrawal where applicable
- [ ] Verify legal/licensing, health claims, child protection, UGC safeguards where relevant
- [ ] Verify target storefront rules, entitlements and current official policy
- [ ] List untested items and owner responsible; do not mark ready until blockers resolved
