# Skill: oss-contribute (Open Source Contribution Workflow)

## Purpose
Reusable checklist for making clean, high-quality open source PRs. Updated after every contribution attempt and every maintainer feedback/error. Always follow this before opening a PR.

## Pre-flight (before writing code)
1. **Search existing PRs** for the issue keyword:
   `gh search prs --repo <owner/repo> --state open "<issue# or title>"`
   If an open PR exists, STOP — do not duplicate.
2. **Search existing issues** to confirm the bug is real and unclaimed:
   `gh issue view <n> --repo <owner/repo>`
3. **Read CONTRIBUTING.md** of the repo. Note test command, commit style, PR template.
4. **Check the issue has no "in progress" / assignee.**

## Branching (CRITICAL — learned from express #7581-#7585 failures)
- Always branch from a CLEAN upstream base:
  `git fetch upstream && git checkout -b fix/<slug> upstream/<default-branch>`
- NEVER branch from a previous feature branch (causes stacked commits in PRs).
- One change per branch. One PR per branch.

## Implementation
- Reproduce the bug first (write a failing test or minimal repro).
- Make the smallest correct fix.
- Add a regression test that fails before the fix and passes after.
- Run the FULL test suite locally (not just the touched file).
- Check for repo-required files: `PRE_RELEASE_CHANGELOG.md`, changesets, etc. (read AGENTS.md / CONTRIBUTING.md).

## Pre-PR checklist (in-depth verification)
- [ ] Diff contains ONLY this PR's change (`git diff upstream/master...HEAD`).
- [ ] No commits from other PRs leaked in.
- [ ] Tests pass locally.
- [ ] Commit message follows repo convention.
- [ ] PR title references the issue: `fix: ... (Fixes #N)`.
- [ ] PR body explains root cause + fix + test.

## Opening the PR
- `gh pr create --base <default> --head <user>:<branch>` with clear title/body.
- Wait for CI. If CI fails, fix on the same branch and push.
- Do NOT open another PR to the same repo until this one is reviewed/closed.

## After feedback
- Address review comments on the SAME branch (push new commits).
- If closed as duplicate: accept gracefully, note the original PR number in this skill.
- If closed as "won't fix": note the reason; don't retry.

## Lessons learned (changelog)
- 2026-11-02: express PRs #7581-#7585 all closed — stacked commits + duplicates. Lesson: clean base branch + search first.
- 2026-11-02: axios PR #11413 open but Greptile found allocation-guard regression, missing tests, missing changelog. Lesson: read AGENTS.md requirements before PR.
- 2026-11-02: prettier issues #20355, #20370, #20378, #20381, #20404, #20220 all already had open PRs or were fixed on main. Lesson: always search PRs before starting.

## Dependencies & Tools (exact)
- **GitHub CLI (`gh`)**: authenticated (`gh auth status`). Used for search, fork, clone, PR. Token scopes: repo, workflow.
- **git**: configured user.name/user.email.
- **Node.js + npm**: for running tests in JS repos.
- **Python 3**: for email skill (IMAP/SMTP).
- **agent-browser**: for web research (optional; may time out on heavy sites).
- **webfetch**: for fetching GitHub issues/PRs/docs (preferred for static pages).
- **email skill**: reads inbox via IMAP (Gmail app password). Used to check PR feedback.
- **OpenCode skills catalog**: load via `skill` tool by id (e.g., `github-engineer`, `code-reviewer`, `test-engineer`).

## Exact workflow (step-by-step commands)
1. Search issues: `gh search issues --repo <owner/repo> --state open --limit 15 --json number,title`
2. Search existing PRs: `gh search prs --repo <owner/repo> --state open "<issue#>"`
3. Fetch issue: `gh issue view <n> --repo <owner/repo>` or webfetch
4. Read CONTRIBUTING.md: `gh repo view <owner/repo> --web` or fetch raw file
5. Fork + clone: `gh repo fork <owner/repo> --clone` (creates upstream remote)
6. Branch from clean base: `git fetch upstream && git checkout -b fix/<slug> upstream/<default-branch>`
7. Reproduce + fix + test (repo-specific test command)
8. Verify diff is clean: `git diff upstream/<default>...HEAD`
9. Commit: `git commit -m "fix: ... (Fixes #N)"`
10. Push: `git push origin fix/<slug>`
11. Open PR: `gh pr create --base <default> --head <user>:fix/<slug> --title "..." --body "Fixes #N ..."`
12. Monitor: `gh pr view <n> --json state,mergeable` and `gh pr checks <n>`
13. Check email for maintainer feedback: use email skill (IMAP).
14. If feedback: push fixes to same branch; reply via `gh pr comment`. Bot reviews (Greptile, etc.) count — address every finding before expecting merge.

## Lessons learned (changelog)
- 2026-11-02: express PRs #7581-#7585 all closed — stacked commits + duplicates. Lesson: clean base branch + search first.
- 2026-11-02: axios PR #11413 open but Greptile found allocation-guard regression, missing tests, missing changelog. Lesson: read AGENTS.md requirements before PR.
- 2026-11-02: prettier issues #20355, #20370, #20378, #20381, #20404, #20220 all already had open PRs or were fixed on main. Lesson: always search PRs before starting.
- 2026-11-02: chalk issue #690 locked as spam (maintainer Qix- called out AI agents). Lesson: some repos are hostile to AI contributions; check maintainer tone / recent issue locks before investing. Prefer repos with welcoming CONTRIBUTING.md and active maintainer engagement.
- 2026-11-02: commander.js #2651 already fixed in develop (commit 1536d5b). Lesson: check `git log` on develop/main for the fix before starting; "pending release" label often means already merged.
- 2026-11-02: zod issues #6713, #6709 have multiple people working / PRs limited to collaborators. Lesson: check if repo accepts external PRs; check issue activity and comments.
- 2026-11-02: commander.js #2651 labeled "pending release" — likely already fixed in develop. Lesson: check labels and recent commits before investing.
- 2026-11-02: axios PR #11413 — Greptile found 3 issues: (1) allocation-guard regression where estimate underestimates raw-body fallback allocation, (2) missing regression tests for invalid percent-escapes, (3) missing PRE_RELEASE_CHANGELOG.md entry. All three fixed: estimate now returns body.length for invalid escapes, added 4 regression tests, added changelog entry. PR still open awaiting re-review/merge.
