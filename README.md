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

### Recommended: universal symlink installation

Keep one checkout of this skill and link it to your agents. Updates and local edits then live in one place.

“Universal” here means available across your local projects in Codex and Claude Code. Other agents need their own supported skill location; this setup does not install into remote or cloud environments.

Requires Git and a macOS or Linux shell.

#### 1. Clone the skill once

```bash
mkdir -p "$HOME/.local/share/agent-skills"
git clone https://github.com/yohannescodes/apple-app-review-compliance.git \
  "$HOME/.local/share/agent-skills/apple-app-review-compliance"
```

If you already have a source checkout, use its absolute path as `skill_source` in the next step.

#### 2. Link it to your agents

The block below installs for both Codex and Claude Code. Remove either destination from the loop if you only use one agent. Existing installations are left untouched and reported for review.

```bash
(
  set -eu
  skill_source="$HOME/.local/share/agent-skills/apple-app-review-compliance"
  test -f "$skill_source/SKILL.md"
  test -f "$skill_source/LICENSE"
  test -f "$skill_source/references/review-matrix.md"

  for skills_dir in "$HOME/.agents/skills" "$HOME/.claude/skills"; do
    mkdir -p "$skills_dir"
    skill_dest="$skills_dir/apple-app-review-compliance"
    if [ -e "$skill_dest" ] || [ -L "$skill_dest" ]; then
      printf 'Already exists; inspect before replacing: %s\n' "$skill_dest"
    else
      ln -s "$skill_source" "$skill_dest"
      printf 'Linked: %s\n' "$skill_dest"
    fi
  done
)
```

This links the entire bundle, including `SKILL.md`, `LICENSE`, `references/`, and `agents/`.

Already installed a copy? Save any customizations, then move the old installation to a backup folder outside all agent skills directories before rerunning step 2. Also check for earlier copies under `~/.codex/skills/` or inside your projects so the same skill is not discovered twice. Keep the backup until discovery succeeds.

#### 3. Confirm discovery and use it

Open the app project you want reviewed and start a fresh agent session.

- **Codex:** type `$` and select `apple-app-review-compliance`.
- **Claude Code:** enter `/apple-app-review-compliance`.

If it does not appear, confirm the link resolves to a folder containing `SKILL.md` and `references/review-matrix.md`. Check for duplicate installations or managed settings that restrict skills.

**Codex prompt:**

```text
Use $apple-app-review-compliance to audit this app for submission.
Identify the reviewed revision, evidence gaps, and release verdict.
Do not modify code yet.
```

**Claude Code prompt:**

```text
/apple-app-review-compliance Audit this app for submission.
Identify the reviewed revision, evidence gaps, and release verdict.
Do not modify code yet.
```

You can also request a PR review, feature planning, or rejection triage. Supply the comparison revision, proposed behavior, or Apple's rejection message and affected build.

Skill locations and symlink support were checked October 17, 2026 against the [Codex documentation](https://developers.openai.com/codex/skills/) and [Claude Code documentation](https://code.claude.com/docs/en/skills).

### Update the shared skill

Review upstream changes in the shared checkout:

```bash
skill_source="$HOME/.local/share/agent-skills/apple-app-review-compliance"
git -C "$skill_source" status --short
git -C "$skill_source" fetch origin
git -C "$skill_source" diff HEAD..@{upstream}
```

After reviewing the changes and saving any local customizations, update:

```bash
git -C "$HOME/.local/share/agent-skills/apple-app-review-compliance" pull --ff-only
```

Every linked agent uses the updated files. Start a fresh session to ensure previously loaded instructions and references are refreshed. Local edits through a link also change the shared checkout.

For a reproducible audit, record the skill's commit and any local changes:

```bash
git -C "$HOME/.local/share/agent-skills/apple-app-review-compliance" rev-parse HEAD
git -C "$HOME/.local/share/agent-skills/apple-app-review-compliance" status --short
```

A commit identifies the exact skill contents only when the working tree is clean.

### Optional: install for one project

To limit discovery to one app, use a project destination in step 2 instead of the user-wide destinations:

| Agent | Project skills directory |
|---|---|
| Codex | `/absolute/path/to/app/.agents/skills` |
| Claude Code | `/absolute/path/to/app/.claude/skills` |

An absolute symlink works on your machine. For a team or cloud environment, include the actual skill bundle (including `LICENSE`) in the project's supported skills directory so it does not depend on your personal filesystem.

### Optional: use a standalone copy

If symlinks are unsuitable, copy the complete skill bundle into the agent's skills directory. Include `SKILL.md`, `LICENSE`, the complete `references/` directory, and `agents/` when present. Preserve the copyright and license notice in redistributed copies. Record the source commit and preserve any local edits separately.

Copies are snapshots. To update, back up the old installation outside the skills directories, then replace the complete bundle, including `LICENSE`, from a reviewed source revision. Verify discovery before removing the backup.

### Remove a symlink installation

Remove only the links you created; the shared checkout remains available:

```bash
for skills_dir in "$HOME/.agents/skills" "$HOME/.claude/skills"; do
  skill_dest="$skills_dir/apple-app-review-compliance"
  if [ -L "$skill_dest" ]; then
    unlink "$skill_dest"
  fi
done
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
├── LICENSE
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── review-matrix.md
```

- [`LICENSE`](LICENSE) — MIT license and copyright notice; include with copied installations.
- [`agents/openai.yaml`](agents/openai.yaml) — optional UI display metadata and default prompt; not required for skill validity.
- [`SKILL.md`](SKILL.md) — agent role, scope, audit workflow, priority checks, and reporting expectations.
- [`references/review-matrix.md`](references/review-matrix.md) — evidence-oriented checklist linking triggers to guideline sections and validation methods.

## Source of truth and limitations

The canonical source is [Apple's App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/). The policy sources were retrieved October 17, 2026; Apple’s guidelines displayed October 16, 2026 as their update date. The [update announcement](https://developer.apple.com/news/?id=92th7e5i) and [account-deletion guidance](https://developer.apple.com/help/app-review/guideline-reference/5-1-1-account-deletion) were also consulted. Apple may change the rules, entitlements, regional exceptions, App Store Connect requirements, or interpretation without updating this repository. Agents should retrieve the live policy for each substantive audit and disclose when they cannot.

Policy compliance is contextual. Static code inspection cannot establish server-side practices, runtime outcomes, regulatory permissions, App Store Connect configuration, or Apple's final review decision. The skill deliberately calls those out as manual checks rather than certifying compliance.

## Contributing

Changes are welcome, particularly corrections tied to an official Apple section and reproducible validation steps. When proposing a guideline update, include the original Apple URL, applicable storefronts/platforms, and date verified. Avoid copying large portions of Apple's copyrighted documentation into this repository.

## License

This repository’s original content is licensed under the [MIT License](LICENSE), copyright © 2026 Yohannes Haile. Include the license and copyright notice when copying or redistributing the skill.

Apple’s guidelines and other third-party materials remain subject to their respective owners’ rights; this license does not grant rights to those materials.
