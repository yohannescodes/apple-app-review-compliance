# Apple App Review Compliance — Agent Skill

A reusable agent skill for reviewing Apple-platform apps against the [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/). Designed for code-review and submission-readiness workflows with Claude Code, Codex, and compatible agents that support `SKILL.md` instructions.

> Independent developer tool. Not affiliated with or endorsed by Apple. An audit does not guarantee App Store approval.

## What it does

- Maps application behavior to Apple's five guideline categories: **Safety, Performance, Business, Design, and Legal**.
- Reviews Swift/SwiftUI code and project configuration, entitlements, permission prompts, third-party SDKs, privacy manifests, and relevant runtime flows.
- Checks in-app purchases, subscriptions, StoreKit restoration, authentication, account deletion, App Privacy disclosures, and submission metadata.
- Applies extra scrutiny to health apps, financial apps, AI services, children’s apps, and user-generated content.
- Produces prioritized findings tied to numbered Apple guideline sections, available evidence, remediation, and verification steps.
- Distinguishes *confirmed violations* from *likely rejection risks* and *manual checks*; does not claim to verify files or App Store Connect settings it cannot access.

## Install

Clone or download this repository, then copy the skill directory into your coding agent's skills folder, retaining the `references/` subdirectory.

**Claude Code** (project scope):

```bash
mkdir -p .claude/skills/apple-app-review-compliance
cp SKILL.md .claude/skills/apple-app-review-compliance/
cp -R references .claude/skills/apple-app-review-compliance/
```

**Codex** (project scope):

```bash
mkdir -p .agents/skills/apple-app-review-compliance
cp SKILL.md .agents/skills/apple-app-review-compliance/
cp -R references .agents/skills/apple-app-review-compliance/
```

For a user-wide installation, use the skills location supported by your agent. Cursor and other agents may have different conventions; consult their current documentation.

## Usage

In your agent session, ask:

```text
Audit this iOS app for App Store review compliance. Inspect the codebase,
privacy manifests, permissions, StoreKit subscriptions and paywalls, account
deletion, and relevant metadata. Cite specific Apple guideline subsections,
show evidence for each finding, rank rejection risks by severity, and
provide a concrete remediation and test plan. Do not modify code yet.
```

To review a specific feature:

```text
Review this PR's changes to HealthKit syncing and subscription onboarding
against current App Review Guidelines. Focus on actual changed behavior,
identify manual verification steps, and suggest minimally invasive fixes.
```

To prepare a release:

```text
Run a pre-submission audit and create an App Store review readiness report.
Separate code-verified checks, metadata/App Store Connect checks, and
unknowns that require device or manual review.
```

## Typical output

| Severity | Classification | Guideline | Evidence | Action |
|---|---|---|---|---|
| High | Likely rejection risk | 3.1.2 | Paywall does not clearly disclose renewal terms | Add required terms and test the purchase path |
| Medium | Requires manual verification | 2.3 | App Store screenshots not provided | Compare listing against the current build |

These are **illustrative findings**, not an assessment of any particular app.

## Repository contents

```text
.
├── README.md
├── SKILL.md
└── references/
    └── review-matrix.md
```

- [`SKILL.md`](SKILL.md) — agent role, scope, audit workflow, priority checks, and reporting expectations.
- [`references/review-matrix.md`](references/review-matrix.md) — evidence-oriented checklist linking triggers to guideline sections and validation methods.

## Source of truth and limitations

The canonical source is [Apple's App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/). The packaged guidance was drafted October 17, 2026 based on the guidelines marked updated October 16, 2026. Apple may change the rules, entitlements, regional exceptions, App Store Connect requirements, or interpretation without updating this repository. Agents should retrieve the live policy for each substantive audit and disclose when they cannot.

Policy compliance is contextual. Static code inspection cannot establish server-side practices, runtime outcomes, regulatory permissions, App Store Connect configuration, or Apple's final review decision. The skill deliberately calls those out as manual checks rather than certifying compliance.

## Contributing

Changes are welcome, particularly corrections tied to an official Apple section and reproducible validation steps. When proposing a guideline update, include the original Apple URL, applicable storefronts/platforms, and date verified. Avoid copying large portions of Apple's copyrighted documentation into this repository.

## License

No license is currently granted for redistribution or reuse beyond rights provided by applicable law. A maintainer may add an explicit open-source license separately.
