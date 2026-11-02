# Skill: oss-contribute — End-to-End Open Source Contribution Workflow

## Purpose
A complete, production-grade workflow for contributing to open source repositories end-to-end — from finding an issue to getting a PR merged. Written from both the contributor and maintainer perspective. Updated after every contribution attempt (success or failure) so it stays a living, reliable guide.

---

## Phase 0: Find the Right Issue

### 1. Pick a welcoming repo
Check before you invest time:
- **CONTRIBUTING.md exists** and is welcoming to outside contributors
- **Recent activity** — last commit within 30 days, issues/PRs getting responses
- **Maintainer engagement** — maintainers reply to issues, review PRs
- **Not collaborator-only** — look for "CreatePullRequest permission error" or "PRs limited to collaborators" in issue comments (means you're blocked)
- **Not locked/spam-locked** — if the maintainer has locked issues calling out bots/AI, stay away
- **Labels** — "help wanted", "good first issue", "beginner-friendly" are good signs; "pending release" often means already fixed

### 2. Search existing work FIRST (critical)
Before writing any code:
```bash
# Search open PRs for the issue keyword
gh search prs --repo <owner/repo> --state open "<issue number or title>"
gh search prs --repo <owner/repo> --state closed "<issue number or title>"
# Search issues
gh search issues --repo <owner/repo> --state open "<keyword>"
# Check if already fixed in default branch
git fetch upstream
git log --oneline upstream/main --grep="<keyword>" | head -5
```
**Stop here if:** an open PR already exists, the issue was already fixed, or multiple people are already working on it.

### 3. Read the issue completely
- Reproduce the bug yourself before touching code
- Check if there's a repro, screenshots, or minimal example
- Note what the maintainer explicitly asked for (or rejected)
- Check `AGENTS.md` if it exists — it often has specific PR requirements (tests, changelog, etc.)

---

## Phase 1: Setup (Clean Base Branch — CRITICAL)

```bash
# 1. Fork + clone (only once per repo)
gh repo fork <owner/repo> --clone
cd <repo>

# 2. Add upstream remote
git remote add upstream https://github.com/<owner/repo>.git

# 3. ALWAYS branch from clean upstream default branch
git fetch upstream
git checkout -b fix/<slug> upstream/<default-branch>
# e.g. git checkout -b fix/subdomains-ipv6 upstream/master
#      git checkout -b fix/fromdatauri-stray-percent upstream/v1.x

# 4. NEVER branch from a previous feature branch
#    (causes stacked commits — maintainers hate this)
```

**Golden rule:** One change per branch. One PR per branch. Never stack PRs.

---

## Phase 2: Implement

### 1. Reproduce first
Write a failing test or minimal repro script that demonstrates the bug. Confirm it fails before your fix.

### 2. Make the smallest correct fix
- Touch the minimum number of files
- Match the existing code style (Prettier, ESLint, Biome, etc.)
- No unrelated refactors, no cosmetic changes
- **If the function you're changing has multiple callers, check ALL of them first** — a fix that's correct for one caller can break another (e.g. `estimateDataURLBytes` is used by both the Node http adapter AND Fetch; fixing one broke the other)
- **For buffer/allocation estimates, always use byte length, not character count** — `str.length` (UTF-16 code units) ≠ `Buffer.byteLength(str, 'utf8')` for non-ASCII strings
- **Keep JSDoc/doc comments directly above their function** — inserting a helper between a JSDoc block and its function detaches the docs (documentation tooling attaches them to the wrong symbol)
- **Every new code branch needs its own regression test** asserting the exact new behavior — a fix without a test for THAT branch is incomplete
- **When validating against runtime behavior, differential-test your logic against the real implementation** — write your validator/predictor, run it against thousands of targeted + fuzz cases comparing to the actual runtime, and require 0 mismatches before pushing
- **Reply to every review thread** (bot or human) with the fix commit and a concise explanation; resolve threads once addressed

### 3. Add a regression test
- Test that fails BEFORE the fix and passes AFTER
- Cover edge cases (lone %, invalid escape, mixed escapes, etc.)
- Match the project's test framework (vitest, jest, mocha, etc.)

### 4. Run the FULL test suite locally
```bash
npm test           # or the repo's test command
npm run check      # lint + typecheck + format
```
**Do not push if any test fails.** Don't assume "it works on my machine" is enough.

### 5. Check repo-specific requirements
Read `AGENTS.md`, `CONTRIBUTING.md`, `PRE_RELEASE_CHANGELOG.md`, changeset config:
- Does the repo require a changelog entry? (`PRE_RELEASE_CHANGELOG.md`)
- Does it require tests for every bug fix? (`AGENTS.md`)
- Does it require docs updates?
- Do NOT update CHANGELOG.md or version numbers if maintainers say they handle that (commander.js example)

---

## Phase 3: Verify Your Diff is Clean

```bash
# See exactly what your PR will contain
git diff upstream/<default-branch>...HEAD

# Verify: ONLY your fix + test + changelog. No commits from other PRs.
# Verify: No debugging console.log, no temp files, no unrelated changes.
```
If you see commits from earlier PRs in your diff, **abort and rebase onto a clean base.** Stacked commits got express PRs closed.

---

## Phase 4: Commit & Push

```bash
git add <changed files>
git commit -m "fix: <concise description> (Fixes #<issue number>)"
git push origin fix/<slug>
```

Commit message conventions vary — match the repo (Conventional Commits: `fix:`, `feat:`, `docs:`, etc.).

---

## Phase 5: Open the PR

```bash
gh pr create \
  --base <default-branch> \
  --head <your-username>:fix/<slug> \
  --title "fix: <concise description>" \
  --body "Fixes #<issue number>

## What changed
- Root cause
- Fix applied
- Test added

## Testing
- Ran full test suite locally
- Added regression test"
```

### PR body should answer:
- What problem are you solving?
- What issue does this relate to? (`Fixes #N`)
- Suggested CHANGELOG summary (if maintainers handle changelogs)
- Any decisions you'd like feedback on

---

## Phase 6: Monitor & Respond (Critical)

```bash
# Check CI status
gh pr checks <pr-number>
gh pr view <pr-number> --json state,mergeable

# Watch for comments
gh pr view <pr-number> --json comments
```

### Handle ALL feedback:
- **Maintainer comments:** Address each one, push to the **same branch**, reply
- **Bot reviews (Greptile, CodeQL, etc.):** These count. Address every finding:
  - Allocation-guard regression → fix the estimate
  - Missing tests → add them
  - Missing changelog → add `PRE_RELEASE_CHANGELOG.md` entry
  - Bot says "not safe to merge until..." → do not consider it done until resolved
- **CI failures:** Fix on the same branch, push, re-check

**Do not open a new PR** to the same repo until the current one is reviewed/closed.

---

## Phase 7: After Merge/Close

### If merged:
- Celebrate. Your contribution graph and Pull Shark badge update.
- Delete your feature branch: `git branch -d fix/<slug>`

### If closed as duplicate:
- Accept gracefully. Note the original PR number. Don't retry.

### If closed as "won't fix":
- Note the reason. Don't argue with maintainers.

### If the maintainer asks you to stop (e.g. "stop creating duplicate PRs"):
- Stop immediately. Review your process. Fix the workflow before contributing again.

---

## Community / Maintainer Perspective (What maintainers actually want)

If you were a maintainer, you'd want contributors to:

1. **Search first** — don't duplicate existing work
2. **Small, focused PRs** — one concern, easy to review in minutes
3. **Tests included** — don't make me write tests for your fix
4. **Clean diff** — no stacked commits, no unrelated changes
5. **Follow CONTRIBUTING.md** — shows respect for project process
6. **Respond to feedback** — don't abandon your PR after opening
7. **Accept decisions** — if I say "won't fix" or "duplicate", move on
8. **Don't spam** — 5 PRs to one repo in an hour looks like a bot

---

## Lessons Learned (Updated After Every Attempt)

| Date | Repo | Outcome | Lesson |
|------|------|---------|--------|
| 2026-11-02 | expressjs/express | 5 PRs closed, none merged | Stacked commits + duplicates + maintainer asked to stop. Clean base branch + search first. |
| 2026-11-02 | axios/axios | PR #11413 open, Greptile findings | Greptile bot reviews count. Fixed: allocation-guard regression, 4 regression tests, changelog entry. **Follow-up 1**: put check only in `estimateDataURLBufferAllocation` (Node), never in `estimateDataURLBytes` (used by Fetch too). **Follow-up 2**: allocation estimates must use **UTF-8 byte length**, not character count — `body.length` underestimates `Buffer.from(body, 'utf8')` for non-ASCII (é = 2 chars = 3 bytes). **Follow-up 3 (cubic)**: JSDoc must sit directly above its function (helpers go before the JSDoc, not between JSDoc and function); every new code branch needs its own regression test asserting the exact new behavior. **Follow-up 4 (Greptile P1, round 4)**: stray-% detection alone is insufficient — syntactically-valid escapes forming invalid UTF-8 (%FF, %C0, overlong %E0%80%80, surrogates %ED%A0%80, truncated %C3) also throw; added a streaming O(1)-space UTF-8 run validator. **Verified with differential testing**: 593 targeted + 5000 fuzz cases vs Node's decodeURIComponent, 0 mismatches, 0 bound violations. Replied to and resolved every bot review thread. Tests 1713 pass. |
| 2026-11-02 | prettier/prettier | Issues already had open PRs or fixed on main | Always search PRs before starting work. |
| 2026-11-02 | chalk/chalk | Issue #690 locked as spam (maintainer called out AI agents) | Some repos are hostile to AI contributions. Check maintainer tone and issue locks before investing. |
| 2026-11-02 | commander.js | Issue #2651 already fixed in develop | Check `git log` on develop/main; "pending release" label often means already merged. |
| 2026-11-02 | colinhacks/zod | PRs limited to collaborators; multiple people working | Check if repo accepts external PRs; check issue activity/comments first. |

---

## Maintenance Rule

**After every contribution attempt** (success, failure, rejection, or bot review):
1. Append a row to "Lessons Learned" above
2. If a new failure mode appears, add it to the relevant phase checklist
3. Push the updated skill to your public skill repo so others benefit

---

## Quick Command Reference

```bash
# Find issues
gh search issues --repo <owner/repo> --state open --limit 20 --json number,title

# Check for existing PRs
gh search prs --repo <owner/repo> --state open "<issue#>"

# Fork + clone
gh repo fork <owner/repo> --clone

# Clean branch
git fetch upstream && git checkout -b fix/<slug> upstream/<default>

# Commit
git commit -m "fix: <desc> (Fixes #N)"

# Push
git push origin fix/<slug>

# Open PR
gh pr create --base <default> --head <user>:fix/<slug> --title "..." --body "..."

# Monitor
gh pr checks <pr-number>
gh pr view <pr-number>
```

---

## Success = Merge, Not Just PR

Opening a PR is not success. **Getting merged is success.** Everything in this workflow is designed to maximize the probability that a maintainer will review, approve, and merge your PR — and want you to contribute again.
