# Claude Code Team Setup

Reference configuration for using Claude Code. Clone this repo, copy the `.claude/` folder into your projects, and get 16 shared skills out of the box.

## Quick start

```bash
# Clone this repo
git clone https://github.com/camerhann/claude-code-setup.git

# Copy skills into your project
cp -r claude-code-setup/.claude/ your-project/.claude/

# Commit so the whole team gets them
cd your-project
git add .claude/
git commit -m "Add Claude Code team skills"
```

Type `/` in Claude Code to see all available skills in the autocomplete.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated
- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated — required for all `gh-*` skills
- A Claude Code Teams subscription

## Personal vs project skills

| Location | Prefix | Who gets them |
|---|---|---|
| `~/.claude/skills/<name>/SKILL.md` | Available everywhere | Just you |
| `your-repo/.claude/skills/<name>/SKILL.md` | Available in that repo | Everyone who clones the repo |

This repo gives you the project version. To use them as personal skills instead, copy into `~/.claude/skills/`.

---

## Skills reference

### Overview

| Skill | What it does | Reads code | Writes code | Posts to GitHub | Confirmation gate |
|---|---|---|---|---|---|
| `/review` | Review staged changes | Yes | No | No | No — read-only analysis |
| `/explain` | Codebase architecture breakdown | Yes | No | No | No — read-only analysis |
| `/test-this` | Generate and run tests | Yes | Yes | No | No — but runs tests to verify |
| `/refactor` | Suggest refactoring improvements | Yes | On approval | No | Yes — asks before changing anything |
| `/debug` | Root cause analysis | Yes | On approval | No | Yes — proposes fix, waits for go-ahead |
| `/security-audit` | OWASP-style security scan | Yes | No | No | No — read-only analysis |
| `/migrate-check` | Review DB migrations | Yes | No | No | No — read-only analysis |
| `/pr-summary` | Generate PR description | Yes | No | No | No — read-only analysis |
| `/bug-hunt` | Scan for production bugs | Yes | No | No | No — asks before next steps |
| `/gh-issue-create` | Create GitHub issues | Yes | No | Yes | Yes — shows issue before posting |
| `/gh-issue-triage` | Audit open issues | No | No | On approval | Yes — asks before any changes |
| `/gh-lint-issues` | Quality gate for issues | No | No | On approval | Yes — asks before any changes |
| `/gh-fix-issue` | Implement a fix for an issue | Yes | Yes | No | Yes — shows plan before coding |
| `/gh-pr-review` | Review PR against its issue | Yes | No | Yes | Yes — shows review before posting |
| `/gh-pr-create` | Create a PR | Yes | No | Yes | Yes — shows PR before creating |
| `/gh-pipeline` | Full bug-to-PR pipeline | Yes | Yes | Yes | Yes — gate at every phase |

---

## Code quality skills

### `/review`

**What it does:** Reviews your staged changes (or unstaged if nothing is staged) for bugs, security issues, and performance problems. Ends with a verdict: SHIP IT, NEEDS FIXES, or HOLD FOR DISCUSSION.

**What it checks:**
- Security — injection, auth bypass, hardcoded secrets, unsafe deserialization, SSRF
- Bugs — logic errors, off-by-ones, null/undefined paths, race conditions
- Performance — N+1 queries, missing indexes, unnecessary loops
- Breaking changes — modified public APIs, changed return types, removed exports
- Missing edge cases — empty inputs, boundary values, concurrent access

**What it ignores:** Style, formatting, naming preferences, import ordering. That's what linters are for.

**Safety:** Read-only. Does not modify any files or post anything externally. Only reads your git diff.

**Best practice:** Run this before every commit. Stage your changes with `git add`, then run `/review`. Fix anything flagged, then commit. This catches issues before they reach a PR.

**Usage:**
```
/review
```

---

### `/refactor`

**What it does:** Analyses a file or module and suggests concrete refactoring improvements. Shows you what it would change and why — then waits for your approval before touching anything.

**What it looks for:**
- Readability — confusing names, deep nesting, functions doing too many things
- Duplication — repeated code that should be extracted
- Complexity — overly long functions with too many branches
- Type safety — missing types, overly broad `any` types, unsafe casts
- Testability — tightly coupled code that's hard to test

**Safety:** Does not change any code until you explicitly confirm which suggestions to apply. You pick and choose.

**Best practice:** Use on files you're about to modify anyway — refactoring before adding a feature makes the feature easier to implement. Don't refactor code you're not working on.

**Usage:**
```
/refactor src/services/billing.ts
/refactor src/api/
```

---

### `/test-this`

**What it does:** Reads a file or function, understands the implementation, generates thorough tests covering happy paths, edge cases, error cases, and integration scenarios. Then runs the tests to make sure they pass.

**What it covers:**
- Happy path — normal expected inputs and outputs
- Edge cases — empty inputs, nulls, zeros, max values, boundary conditions
- Error cases — invalid inputs, network failures, missing data
- Integration — external service and database interactions

**What it won't do:** Write trivial `assert true` tests, over-mock to the point tests don't verify real behaviour, or skip error paths.

**Safety:** Creates new test files and runs the test suite. Does not modify your source code. Uses whatever testing framework your project already uses (reads package.json, pyproject.toml, etc.).

**Best practice:** Use this after implementing a feature or fix, before creating a PR. Point it at the file you changed. Review the generated tests — they're a starting point, not a finished product. Remove any that don't add value.

**Usage:**
```
/test-this src/services/auth.ts
/test-this src/utils/validation.py
```

---

### `/debug`

**What it does:** Takes an error message, stack trace, or symptom description and systematically finds the root cause. Forms hypotheses, reads the code, verifies, then proposes a fix.

**Methodology:**
1. Understand the symptom
2. Locate the relevant code and trace execution
3. Form 2-3 hypotheses for the root cause
4. Read surrounding code and verify
5. Propose a targeted fix with explanation
6. Suggest a regression test

**Safety:** Read-only investigation. Proposes a fix but waits for you to approve before making changes. Does not modify code without your go-ahead.

**Best practice:** Paste the full error message or stack trace — more context means a better diagnosis. If you're describing a symptom ("the API sometimes returns stale data"), be specific about when it happens.

**Usage:**
```
/debug "TypeError: Cannot read property 'id' of undefined at UserService.getProfile"
/debug "API returns 200 but the response body is empty when the user has no orders"
```

---

### `/security-audit`

**What it does:** Performs an OWASP-style security audit of your codebase. Each finding includes severity (CRITICAL/HIGH/MEDIUM/LOW), exact file location, exploitation description, and a concrete fix.

**What it checks:**
- Injection — SQL, command, XSS, template, LDAP
- Auth — missing checks, broken access control, privilege escalation
- Secrets — hardcoded keys, passwords, tokens in source and config
- Data exposure — sensitive data in logs, error messages, API responses
- Dependencies — known vulnerable packages in lock files
- Cryptography — weak algorithms, bad key lengths, improper IV/nonce usage
- Input validation — missing validation at system boundaries
- SSRF / open redirects — unvalidated URLs, user-controlled redirects

**Safety:** Read-only. Does not modify code or post results anywhere. The findings stay in your terminal.

**Best practice:** Run before any major release or when onboarding a new project. Focus on CRITICAL and HIGH findings first. Pair with `/gh-issue-create` to turn findings into tracked issues.

**Usage:**
```
/security-audit
/security-audit src/api/
/security-audit src/auth/middleware.ts
```

---

### `/migrate-check`

**What it does:** Reviews database migration files for common risks that cause outages during deploys.

**What it checks:**
- Reversibility — does every migration have a rollback?
- Data safety — could this lose data? (dropping columns, truncating, type changes)
- Lock risk — will this lock large tables? (indexes without CONCURRENTLY, ALTER TABLE)
- Compatibility — can old code still work while the migration runs? (zero-downtime deploys)
- Idempotency — will it fail if run twice? (missing IF NOT EXISTS)
- Default values — NOT NULL columns added without defaults

**Safety:** Read-only. Analyses migration files without executing them.

**Best practice:** Run on every PR that includes migrations. Catches the issues that cause 3am pages — table locks on large tables, irreversible data loss, deploy ordering problems.

**Usage:**
```
/migrate-check
/migrate-check db/migrations/
```

---

## Understanding skills

### `/explain`

**What it does:** Gives you a comprehensive architecture overview of a codebase by reading the actual source files. Covers what it does, how it's structured, entry points, data flow, external dependencies, and how to build/test/run it locally.

**Safety:** Read-only. Only reads files — doesn't modify, build, or execute anything.

**Best practice:** Run this when you first enter an unfamiliar repo, or when onboarding a new team member. The output is a solid starting point for a conversation about the codebase. Run it in front of a sceptical colleague — watching Claude read 20+ files and produce a coherent architectural summary is a convincing demo.

**Usage:**
```
/explain
```

---

### `/pr-summary`

**What it does:** Reads all commits and the full diff for your current branch, then generates a structured PR description with summary, grouped changes, testing notes, and risk assessment.

**Safety:** Read-only. Does not create a PR or push anything. Just generates the text.

**Best practice:** Use this to draft your PR description, then review and edit it. It's faster than writing from scratch and ensures you don't miss anything. If you want to go straight to creating the PR, use `/gh-pr-create` instead.

**Usage:**
```
/pr-summary
```

---

## GitHub skills

> **All GitHub skills require the `gh` CLI to be installed and authenticated.** Run `gh auth status` to check.

### `/bug-hunt`

**What it does:** Deep scan of the codebase for real bugs that could cause production incidents. Not style issues — logic errors, race conditions, resource leaks, injection vulnerabilities, unhandled error paths.

**Output format:** Each finding gets a unique ID (BUG-001, BUG-002...) with severity, file location, description, reproduction scenario, and suggested fix.

**Safety:** Read-only. Does not modify any code. After presenting findings, asks whether you want to create GitHub issues or fix them directly — nothing happens without your explicit choice.

**Best practice:** Run on a feature branch, not main. Use it to find bugs in code you're about to release. Cherry-pick the findings that matter and use `/gh-issue-create` to track them, or `/gh-fix-issue` to fix them immediately.

**Usage:**
```
/bug-hunt
/bug-hunt src/api/
```

---

### `/gh-issue-create`

**What it does:** Creates a well-structured GitHub issue with problem statement, reproduction steps, impact assessment, suggested approach, and acceptance criteria. Applies labels from the repo's existing label set.

**Safety:** Shows you the full issue before posting. Nothing is created on GitHub until you explicitly confirm. You can edit the title, body, or labels before confirming.

**Best practice:** Use after `/bug-hunt` to turn findings into tracked issues, or when you spot a problem but don't have time to fix it now. Good issues have clear acceptance criteria — if the criteria are vague, the PR will be vague too.

**Usage:**
```
/gh-issue-create "API returns 500 when email contains unicode characters"
/gh-issue-create   # then describe the issue in conversation
```

---

### `/gh-issue-triage`

**What it does:** Pulls the open issue backlog and assesses each issue for completeness, labelling, duplicates, staleness, and missing comments. Produces a triage report grouped by action needed.

**Report sections:**
- Needs more detail — issues that can't be worked on without clarification
- Missing labels — issues that should be categorised
- Potential duplicates — groups of issues that might be the same problem
- Stale — no activity in 30+ days
- Ready to work — well-defined issues that could be picked up now

**Safety:** Read-only by default. After presenting the report, asks which actions you want to take. Can post comments, add labels, or close stale issues — but only on your explicit confirmation, one action at a time.

**Best practice:** Run weekly to keep the backlog healthy. A clean backlog means the team always knows what to work on next. Pair with `/gh-lint-issues` for a more quantitative quality report.

**Usage:**
```
/gh-issue-triage
```

---

### `/gh-lint-issues`

**What it does:** Audits every open issue against a quality checklist and produces a report card with pass/fail percentages. More structured and quantitative than `/gh-issue-triage`.

**Quality gates checked:**
- Title quality — specific and descriptive, indicates component/area
- Body quality — clear problem statement, reproduction steps or acceptance criteria
- Labels — at least one label applied
- Comments — issues >7 days old have at least one comment
- Staleness — flags 30+ day inactive and 90+ day close candidates

**Safety:** Read-only analysis. Presents a report and then asks which issues you want to fix. Can post comments, add labels, or close issues via `gh` — but only on explicit confirmation per action.

**Best practice:** Run this before sprint planning to ensure all issues in the backlog are actionable. An issue without reproduction steps or acceptance criteria will produce a vague PR — catch that early.

**Usage:**
```
/gh-lint-issues
```

---

### `/gh-fix-issue`

**What it does:** Reads a GitHub issue (including all comments), investigates the codebase, plans a fix, implements it on a feature branch, adds/updates tests, and runs the test suite.

**Workflow:**
1. Read the issue and all comments for full context
2. Investigate — find relevant code, check git blame, read related tests
3. Present a fix plan (root cause, proposed fix, files affected, risks) — **waits for your approval**
4. Create a feature branch (`fix/issue-N-short-description`)
5. Implement the minimum change needed
6. Add or update tests
7. Run the test suite
8. Show a summary and ask about next steps (commit, create PR, adjust)

**Safety:** Creates a feature branch — your main branch is never touched. Shows the plan before writing any code. After implementation, asks what you want to do next rather than pushing or creating a PR automatically.

**Best practice:** Always work on a branch. This skill creates one automatically. Review the plan before approving — if the approach is wrong, it's cheaper to correct the plan than to undo the implementation. After the fix, use `/review` to self-check before creating a PR.

**Usage:**
```
/gh-fix-issue 42
/gh-fix-issue 15
```

---

### `/gh-pr-review`

**What it does:** Reviews a pull request by first reading the linked issue to understand what the PR is supposed to solve, then reviewing every change against that scope. Flags scope creep, incomplete fixes, and unrelated changes — not just code quality.

**Review process:**
1. Fetch the PR and find the linked issue (`Closes #N`, `Fixes #N`)
2. Read the issue and ALL comments to understand the full scope
3. Classify every change as: on-scope, necessary supporting change, scope creep, or unrelated
4. Review on-scope changes for correctness, security, performance, test coverage
5. Check completeness — does the PR address all acceptance criteria from the issue?

**Verdicts:**
- **SHIP IT** — tight fix, fully addresses the issue, no scope creep
- **TRIM AND SHIP** — fix is good but has scope creep that should be a separate PR
- **NEEDS FIXES** — on-scope code has bugs or security issues
- **INCOMPLETE** — doesn't fully address the issue
- **SPLIT THIS PR** — too much going on, break it up

**Safety:** Shows you the full review before posting anything to GitHub. Posts inline comments and an overall review only after your go-ahead. You can edit or remove any comment before it's posted.

**Best practice:** Use this on every PR before human review. It catches scope creep that humans tend to wave through ("it's already here, might as well merge it"). A tight PR is easier to review, easier to revert, and easier to understand in git history. If a PR gets TRIM AND SHIP, ask the author to move the extra changes to a follow-up PR.

**Usage:**
```
/gh-pr-review 45
/gh-pr-review 12
```

---

### `/gh-pr-create`

**What it does:** Creates a pull request from your current branch. Reads all commits and the full diff, generates a structured PR description, suggests labels and reviewers, then creates it via `gh pr create`.

**PR structure:**
- Summary — 2-4 bullets on what and why (business impact first)
- Changes — grouped by area
- Testing — how it was tested, what needs manual verification
- Risks — breaking changes, migration steps, rollback plan
- Related issues — linked with `Closes #N`

**Safety:**
- Refuses to create a PR from main/master
- Warns about uncommitted changes
- Shows the full PR (title, body, labels, reviewers) before creating
- Pushes to remote only if needed, and only with your confirmation

**Best practice:** Run `/review` first to catch issues before the PR exists. Use on a feature branch — never on main. Ensure your branch is up to date with the base branch before creating the PR.

**Usage:**
```
/gh-pr-create
```

---

### `/gh-pipeline`

**What it does:** Runs the full bug-to-PR pipeline: hunt for bugs, create issues, implement fixes, self-review, and ship PRs. Each phase has a confirmation gate — nothing proceeds without your approval.

**Phases:**
1. **Hunt** — scan for bugs → show findings → you pick which to proceed with
2. **Issues** — draft GitHub issues → show them → you confirm before posting
3. **Fix** — create branch, implement, test → show diff → you approve
4. **Review** — self-review for security, performance, coverage → show findings → fix issues
5. **Ship** — commit, create PR with `Closes #N` → show PR → you confirm

**Safety:** This is the most powerful skill but also the most guarded. Every single phase stops and asks for your approval. You can say "stop" at any point to exit, or "skip" to jump to the next phase. Nothing is posted to GitHub, committed, or pushed without explicit confirmation.

**Best practice:** Use this for a demo of the full workflow — it's the showstopper. For daily work, you'll usually run individual skills. Use `/gh-pipeline` when you want to do a sweep of a module or feature area and fix multiple issues in one session. Always review the generated issues and PRs carefully — the pipeline is fast but your judgment is the quality gate.

**Usage:**
```
/gh-pipeline
/gh-pipeline src/api/
```

---

## Best practices

### Always work on a branch

Skills that modify code (`/gh-fix-issue`, `/gh-pipeline`, `/test-this`) should be run on a feature branch, not main. `/gh-fix-issue` creates a branch automatically. For others, create one first:

```bash
git checkout -b feature/your-change
```

### Recommended workflow

```
1. /explain                    # understand the codebase (if new)
2. /bug-hunt                   # find issues
3. /gh-issue-create            # track them
4. /gh-fix-issue 42            # implement a fix (creates a branch)
5. /review                     # self-check before PR
6. /test-this src/changed.ts   # add test coverage
7. /gh-pr-create               # create the PR
8. /gh-pr-review 15            # review someone else's PR
```

You don't need all of these every time. Pick the ones that fit your task.

### What can't go wrong?

These skills are read-only and completely safe to run at any time:
- `/explain`, `/review`, `/pr-summary`, `/security-audit`, `/migrate-check`, `/bug-hunt`

These skills modify local files but not GitHub:
- `/test-this`, `/debug`, `/refactor`

These skills can post to GitHub, but always ask first:
- `/gh-issue-create`, `/gh-issue-triage`, `/gh-lint-issues`, `/gh-pr-review`, `/gh-pr-create`, `/gh-fix-issue`, `/gh-pipeline`

**No skill will ever push to main, force-push, delete branches, or modify GitHub without showing you exactly what it will do and waiting for confirmation.**

### Customising for your project

Edit `CLAUDE.md` in your project root to add project-specific conventions. Claude Code reads this file automatically and follows whatever you put in it — coding standards, build commands, architecture decisions, things to avoid.
