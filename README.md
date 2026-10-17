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

The **source repository** holds this skill. The **destination project** is the app you want to review. Use absolute paths and keep them separate. These examples require Git and a POSIX shell (macOS/Linux); edit the two paths first.

```bash
skill_source="$HOME/Developer Tools/apple-app-review-compliance"
app_project="$HOME/Projects/My App"
mkdir -p "$(dirname "$skill_source")"
git clone https://github.com/yohannescodes/apple-app-review-compliance.git "$skill_source"
# The destination app must already exist.
test -d "$app_project"
```

Choose **one** destination; use the same choice when updating:

| Agent | Project installation | User-wide installation |
|---|---|---|
| Codex | `<project>/.agents/skills/apple-app-review-compliance/` | `~/.agents/skills/apple-app-review-compliance/` |
| Claude Code | `<project>/.claude/skills/apple-app-review-compliance/` | `~/.claude/skills/apple-app-review-compliance/` |

```bash
skill_dest="$app_project/.agents/skills/apple-app-review-compliance"
# Alternatives (uncomment only one):
# skill_dest="$HOME/.agents/skills/apple-app-review-compliance"
# skill_dest="$app_project/.claude/skills/apple-app-review-compliance"
# skill_dest="$HOME/.claude/skills/apple-app-review-compliance"
```

Run this installation block after selecting the paths. It refuses destinations inside the source checkout and existing installations. It copies the entire bundle, including optional `agents/`, and records its source revision.

```bash
(
  set -eu
  skill_source="$(cd "$skill_source" && pwd -P)"
  test -f "$skill_source/SKILL.md"
  test -d "$skill_source/references"
  case "$skill_dest" in /*) ;; *) echo "Use an absolute destination" >&2; exit 1 ;; esac
  mkdir -p "$(dirname "$skill_dest")"
  skill_parent="$(cd "$(dirname "$skill_dest")" && pwd -P)"
  skill_dest="$skill_parent/$(basename "$skill_dest")"
  case "$skill_dest/" in "$skill_source/"*) echo "Destination is inside source" >&2; exit 1 ;; esac
  test ! -e "$skill_dest" && test ! -L "$skill_dest"
  mkdir "$skill_dest"
  cp "$skill_source/SKILL.md" "$skill_dest/"
  cp -R "$skill_source/references" "$skill_dest/"
  if [ -d "$skill_source/agents" ]; then
    cp -R "$skill_source/agents" "$skill_dest/"
  fi
  git -C "$skill_source" rev-parse HEAD > "$skill_dest/SOURCE_COMMIT"
  git -C "$skill_source" status --porcelain > "$skill_dest/SOURCE_WORKTREE_STATUS"
  test -f "$skill_dest/references/review-matrix.md"
)
```

A nonempty `SOURCE_WORKTREE_STATUS` means the copy includes local source edits: the commit alone does not reproduce it. Keep those edits or a patch alongside your update records.

### Discover and invoke

Open the destination app project in your agent. Check the copied `SKILL.md` and `references/review-matrix.md` exist, then verify discovery:

- **Codex CLI/IDE:** use `/skills` or type `$` and select `apple-app-review-compliance`. Changes are detected automatically; restart Codex if it does not appear. Check for duplicate installations or disabled skill settings.
- **Claude Code:** use `/skills` and confirm the skill is listed. Local `SKILL.md` changes are watched except in bare mode. If the top-level skills directory was created after session start, run `/reload-skills` after installation and later changes there. Start a fresh session if already-loaded instructions or companion references remain stale.

Verified October 17, 2026 against [OpenAI’s skill documentation](https://developers.openai.com/codex/skills/) and [Claude Code’s skill documentation](https://code.claude.com/docs/en/skills). Client versions and managed settings can affect discovery.

Codex prompt:

```text
Use $apple-app-review-compliance to audit this app for submission.
Identify the reviewed revision, evidence gaps, and release verdict.
Do not modify code yet.
```

Claude Code prompt:

```text
/apple-app-review-compliance Review this PR's authentication changes.
Compare main...HEAD, identify policy risks and evidence gaps, and suggest fixes.
```

You can also ask for feature planning or rejection triage; supply the proposed behavior or Apple’s rejection message and affected build. The skill infers the mode and scales report depth accordingly.

### Update a copied installation

Copies are snapshots, not linked to upstream. Before updating, read the installed `SOURCE_COMMIT` and `SOURCE_WORKTREE_STATUS`, compare the installed bundle against that revision, and save/review local customizations. Review upstream changes in the source checkout before choosing a new revision (for example, fetch and inspect a diff, then use `git pull --ff-only` if appropriate).

Move the old installation to a backup **outside any agent skills directory**, using a unique absolute backup path:

```bash
skill_backup="$HOME/Skill Backups/apple-app-review-compliance-before-update"
(
  set -eu
  test -d "$skill_dest"
  test ! -e "$skill_backup"
  mkdir -p "$(dirname "$skill_backup")"
  mv "$skill_dest" "$skill_backup"
)
```

Rerun the installation block with the reviewed source and original destination. Replacing the full bundle removes obsolete references and metadata; copying over the old directory can leave stale files. Reapply intentional customizations, record them separately, and repeat discovery verification. If installation fails, retain the backup and restore it after moving any partial installation aside.

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
├── agents/
│   └── openai.yaml
└── references/
    └── review-matrix.md
```

- [`agents/openai.yaml`](agents/openai.yaml) — optional UI display metadata and default prompt; not required for skill validity.
- [`SKILL.md`](SKILL.md) — agent role, scope, audit workflow, priority checks, and reporting expectations.
- [`references/review-matrix.md`](references/review-matrix.md) — evidence-oriented checklist linking triggers to guideline sections and validation methods.

## Source of truth and limitations

The canonical source is [Apple's App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/). The policy sources were retrieved October 17, 2026; Apple’s guidelines displayed October 16, 2026 as their update date. The [update announcement](https://developer.apple.com/news/?id=92th7e5i) and [account-deletion guidance](https://developer.apple.com/help/app-review/guideline-reference/5-1-1-account-deletion) were also consulted. Apple may change the rules, entitlements, regional exceptions, App Store Connect requirements, or interpretation without updating this repository. Agents should retrieve the live policy for each substantive audit and disclose when they cannot.

Policy compliance is contextual. Static code inspection cannot establish server-side practices, runtime outcomes, regulatory permissions, App Store Connect configuration, or Apple's final review decision. The skill deliberately calls those out as manual checks rather than certifying compliance.

## Contributing

Changes are welcome, particularly corrections tied to an official Apple section and reproducible validation steps. When proposing a guideline update, include the original Apple URL, applicable storefronts/platforms, and date verified. Avoid copying large portions of Apple's copyrighted documentation into this repository.

## License

No license is currently granted for redistribution or reuse beyond rights provided by applicable law. A maintainer may add an explicit open-source license separately.
