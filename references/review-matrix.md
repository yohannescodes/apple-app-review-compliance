# App Review evidence matrix

Policy sources retrieved October 17, 2026: [guidelines](https://developer.apple.com/app-store/review/guidelines/) (observed update date October 16, 2026), [update announcement](https://developer.apple.com/news/?id=92th7e5i) (published October 16, 2026), and [account-deletion guidance](https://developer.apple.com/help/app-review/guideline-reference/5-1-1-account-deletion) (no update date observed). These are packaged snapshot provenance, not evidence of a future audit’s live retrieval. Use the policy header and status definitions in [SKILL.md](../SKILL.md).

| Rule | Trigger | Evidence to inspect | Typical validation |
|---|---|---|---|
| 1.2 | content creation/generation/upload/contribution, including UGC, chat and AI creation (sharing not required) | reporting/block/mute/moderation flows, contact | report/block abusive content in production-like environment |
| 1.3 / 5.1.4 | child-directed experience | category, ads SDKs, consent, parental gates | age-appropriate walkthrough |
| 1.4 | health claims, disease decisions, sensor readings | wording, calculation code, clinical basis, disclaimers | compare accuracy claims to sources; test harmful advice paths |
| 2.1 | every submission | startup, links, reviewer demo, backend and IAP visibility | fresh-install E2E on supported OS |
| 2.3 | every submission | listing metadata, screenshots, What's New, age rating | match live UI to every public claim |
| [2.3.14](https://developer.apple.com/app-store/review/guidelines/#accurate-metadata) | creative assets | branding, claims, pricing, audience | inspect supplied assets against current rule |
| [2.5.19](https://developer.apple.com/app-store/review/guidelines/#software-requirements) | every submission | feature flags, dormant code, reviewer access | compare accessible behavior with documented features |
| 2.5 | platform APIs, background work, recording, SDKs | entitlements, Info.plist, frameworks, permissions | check purpose text and background behavior on device |
| 3.1.1 | premium digital features and purchases | StoreKit, purchase links, regions, entitlement | sandbox purchase, restore, offline/relaunch behavior |
| 3.1.2 | auto-renew subscriptions | paywall, product config, terms, upgrade paths | billing disclosures, renewal/cancel flow, entitlement state |
| 3.2.1(viii) | financial trading, investing or money-management services | service provider and territorial licenses | verify submitting financial institution and permissions |
| 5.1.1(ix) | regulated services or sensitive user information | providing legal entity and submission identity | verify entity requirement independently of financial licensing |
| 4.1–4.3 | new apps and updates | differentiation, UX, branding, bundle variants | verify original meaningful app functionality |
| 4.8 | third-party or social login creating/authenticating the primary account | auth flow, alternatives, applicable exceptions | equivalent sign-in or supported exception |
| 5.1.1 | all apps | privacy policy URL, SDKs, purpose strings, deletion | test withdrawal/deletion and policy availability |
| 5.1.2 | tracking, analytics, third-party AI | SDK/network flow, permission/ATT, AI disclosure | verify purpose-specific consent before sharing |
| 5.1.3 | health, fitness or medical data, including outside HealthKit; research where relevant | data provenance, read/write types, cloud sync, sharing recipients, research consent/ethics approval | no fabricated HealthKit data; sensitive data handled correctly |
| 5.2 | third-party brands/APIs/assets | license files, API terms, graphics, music | verify rights and trademark permissions |
| 5.6 | ratings/reviews/growth | review prompts, incentivization | system-supported rating prompt, no manipulation |

## Specific pitfalls

- Distinguish **health data in HealthKit** from **health data stored elsewhere**; Apple specifically prohibits storing personal health information in iCloud under 5.1.3(ii).
- Distinguish **financial record-keeping** from **licensed financial services**. Avoid assuming a personal ledger app is a bank or that every finance app must be operated by a bank; investigate actual service and policy applicability.
- Third-party AI processing of personal data needs explicit disclosure and permission under 5.1.2(i).
- Do not conflate all external payment links: region, type of app, entitlement, and applicable policies matter; verify on the day of review.
- A watchOS complication, widget, App Clip, extension or App Intent must match main-app functionality and its specific policies.
- App Privacy label accuracy and privacy-manifest implementation require inspection of dependencies **and** data practices, not keyword scans alone.
- Apple guideline 4.8 describes privacy properties of an equivalent sign-in service and exceptions; assess exact conditions rather than blindly requiring one brand. Check limited identity collection, private email and consent for advertising interactions. Verify any claimed exception against the current rule: own-account-only systems, marketplace-specific login, existing education/enterprise accounts, government/industry electronic ID, or a client requiring direct access to a specific third-party service.

## Account deletion — 5.1.1(v)

Apply to account creation, including guest/automatic accounts and web registration, in new apps and updates. Use [Apple’s detailed guidance](https://developer.apple.com/help/app-review/guideline-reference/5-1-1-account-deletion):

- Verify easy-to-find in-app initiation everywhere the app is offered; confirmation/reauthentication must not obstruct deletion.
- Check completion removes the account, associated data and shared UGC, with disclosed legally required retention. Deactivation alone is insufficient.
- If completion uses a website, link directly to deletion; tvOS may use a direct URL/QR code. Mandatory support flows need the regulated-industry exception.
- For delayed/manual processing, disclose timing and confirm completion. Inspect backend evidence, not only the button.
- For Sign in with Apple, verify token revocation and relevant account-change notifications.
- Explain continuing subscription billing, request cancellation and provide subscription management; offer refund guidance where relevant. Optional deletion at subscription expiry must preserve immediate deletion. Do not treat account deletion as automatic subscription cancellation.

Record each result and its evidence separately; source inspection alone cannot verify server-side erasure.

## Release checklist

For submission audits, copy this table into the report. Replace “Requires manual verification” only after reviewing evidence or establishing inapplicability. Mark user-excluded/unreviewed rows in the separate scope ledger; retain any resulting critical evidence gap in the verdict. Use only the five statuses defined in SKILL.md.

| Check | Result | Evidence / validation performed | Remaining action / critical unknown |
|---|---|---|---|
| Cold launch, upgrade, lifecycle, offline, denied permissions, supported devices | Requires manual verification | Pending | Establish build coverage |
| Complete build, working URLs/backend, no placeholders | Requires manual verification | Pending | Establish reviewability |
| Reviewer access to gated features, IAP and test data | Requires manual verification | Pending | Supply secure access setup |
| Listing, screenshots, creative assets, age rating, What's New match build | Requires manual verification | Pending | Inspect submitted metadata |
| Purchase/restore, subscription disclosures and entitlement behavior | Requires manual verification | Pending | Establish applicability and billing evidence |
| Privacy policy, App Privacy labels, manifests, SDK/data practices | Requires manual verification | Pending | Reconcile declarations with behavior |
| Account deletion, logout and consent withdrawal | Requires manual verification | Pending | Verify applicability and completion |
| Entity/licenses, health claims, child protection and content safeguards | Requires manual verification | Pending | Resolve relevant policy triggers |
| Storefront rules, entitlements and current policy | Requires manual verification | Pending | Verify applicable exceptions |
| Reviewer notes, dependencies, regional availability | Requires manual verification | Pending | Supply accurate submission notes |

Apply the verdict definitions from SKILL.md: material blockers mean **Not ready**; otherwise unresolved submission-critical evidence means **Undetermined**. **Ready (within stated verified scope)** requires evidenced critical checks, supported applicability decisions and no material unresolved risks. Record owners/next actions for unknowns. A completed source scan or unchecked checklist does not establish readiness.
