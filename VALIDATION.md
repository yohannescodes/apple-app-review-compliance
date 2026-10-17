# Validation record — October 17, 2026

Starting checkout: clean, commit `11ad2f1c446fd37b530806488d611b4b1ff46bdd`. No newer local commits or uncommitted work were present.

## Checks performed

- Read the live official Apple guidelines, October 16 update announcement, and detailed account-deletion guidance linked in the README. The news page initially failed to load; a retry succeeded. Apple’s guidelines displayed October 16, 2026 as their update date.
- Read official Codex and Claude Code skill documentation for locations, invocation, discovery and reload behavior.
- Ran the bundled skill-creator `quick_validate.py`: passed.
- Parsed frontmatter and `agents/openai.yaml` with PyYAML; checked name, description, UI description length and explicit skill mention: passed.
- Resolved relative Markdown resource links: passed.
- Executed the README installation and backup blocks using Bash in temporary, separate source, app and user-home directories with spaces. The source was cloned from the local Git repository and overlaid with the edited bundle; this did not test a fresh network clone from GitHub.
- Tested four fresh installs and four updates (both agents, both scopes). Compared bundle contents, checked recorded commit/dirty state, preserved local customizations in backups, and verified obsolete references disappeared after update: passed.
- Tested refusal to overwrite an existing installation and refusal of a destination inside the source. Tested installation with optional `agents/` absent: passed.
- Ran `git diff --check`: passed.

## Scenario walkthroughs

These are manual instruction reviews against concrete scenarios, not claims that independent agents or an app runtime were tested.

| Scenario | Reviewed outcome |
|---|---|
| Live retrieval succeeds | Header records successful retrieval date, observed Apple date and only sources actually read; verification remains limited to inspected sections. |
| Live retrieval unavailable | Header uses snapshot-only, records failed attempt/reason and local snapshot provenance; current exceptions remain unresolved where material. |
| Focused authentication PR | PR mode records base/head, screens all five areas, investigates affected behavior and leaves whole-submission readiness unassessed. |
| Submission lacks build/metadata | Critical checks remain Requires manual verification; verdict is Undetermined, or Not ready if a material blocker is also known. |
| Inapplicable versus unreviewed | Evidence that a rule’s trigger is absent supports Not applicable. User-excluded payments go under Outside scope / Not reviewed, with any critical gap retained. |
| Proposed versus performed test | A suggested sandbox purchase test stays under Suggested validation. Only an executed action with environment and result enters Validation performed. |

## Remaining verification and decisions

- Actual discovery, invocation and hot reload in installed Codex/Claude Code clients were not exercised; instructions were checked against official documentation.
- No app binary, device behavior or App Store Connect submission was audited by this documentation task.
- Licensing remains the maintainer’s choice; no license was added or changed.

## README symlink revision — October 17, 2026

Applied the maintainer-supplied README verbatim, starting from clean commit `77baffc`.

- Rechecked the official [Codex skill docs](https://developers.openai.com/codex/skills/) and [Claude Code skill docs](https://code.claude.com/docs/en/skills): both document personal/project skill locations and symlinked skill folders. Invocation and fresh-session guidance agree with the documentation.
- Executed the README shell blocks in both Bash and zsh using temporary source, user and project paths containing spaces. Replaced home-path references with temporary paths and the GitHub clone URL with a local test upstream; no real user installations were modified.
- Verified fresh links expose SKILL.md, references and optional metadata; reruns preserve existing installations; real directories and dangling links are reported without replacement.
- Exercised fetch/diff, fast-forward pull and revision/status commands against a new local upstream commit. Both agent links exposed the updated contents.
- Verified unlink removes links (including dangling links), preserves real directories and leaves the source checkout intact. Verified project-scoped destinations for both agents.
- Checked relative README links and `git diff --check`: passed.

The earlier copy-install results above describe the previous README. This revision's tests cover the shared symlink workflow. Network cloning and actual discovery/invocation in agent clients were not tested; client support was verified from official documentation. Licensing is unchanged.

## MIT license adoption — October 17, 2026

The maintainer chose MIT, superseding the earlier pending-license notes. Added the standard MIT text with copyright 2026 Yohannes Haile. Updated the README license section, repository inventory, symlink prerequisite and copy/update guidance to include LICENSE; third-party materials retain their owners’ rights.

Verified that temporary complete copied and symlinked bundles expose the unchanged LICENSE, including paths with spaces. Checked all README shell blocks with Bash and zsh syntax validation, resolved relative README links, and ran `git diff --check`: passed. No client discovery retest was needed for this license/documentation change.
