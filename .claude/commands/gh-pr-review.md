Review a GitHub pull request thoroughly, with a focus on whether the changes actually solve the linked issue — nothing more, nothing less.

## Step 1: Establish the scope

1. Fetch PR details: `gh pr view $ARGUMENTS --json title,body,files,additions,deletions,baseRefName,headRefName,closingIssuesReferences`
2. Find the linked issue — check the PR body for `Closes #N`, `Fixes #N`, `Related to #N`, or issue URLs. Also check `closingIssuesReferences` from the PR JSON.
3. If an issue is linked, read it fully: `gh issue view N --json title,body,comments`. Read ALL comments — they often contain clarification, updated requirements, or scope decisions.
4. If no issue is linked, flag this immediately — every PR should trace back to an issue or documented reason.

From the issue, extract:
- **The specific problem** being reported or feature being requested
- **Acceptance criteria** if any
- **Scope boundaries** — what was explicitly asked for and nothing more

## Step 2: Review the diff against the issue scope

Get the diff: `gh pr diff $ARGUMENTS`
For files needing more context, read the full file.
Check CI: `gh pr checks $ARGUMENTS`

For every changed file and every hunk, ask: **"Does this change directly serve the fix/feature described in the issue?"**

Categorise each change as:
- **On-scope**: Directly addresses the issue
- **Necessary supporting change**: Required to make the fix work (e.g., a type change needed by the fix) — but explain why
- **Scope creep**: Refactoring, cleanup, style changes, feature additions, or "while I'm here" improvements that are NOT required by the issue
- **Unrelated**: Changes to files or logic that have nothing to do with the issue

## Step 3: Quality review (on-scope changes only)

For the on-scope changes, review for:
- **Correctness** — Does this actually fix the issue? Logic errors, missed edge cases, incorrect assumptions
- **Completeness** — Does it address ALL the acceptance criteria and scenarios in the issue?
- **Security** — Injection, auth issues, data exposure, secrets in code
- **Performance** — N+1 queries, unnecessary computation, missing caching where needed
- **Breaking changes** — API contract changes, schema changes, removed exports
- **Test coverage** — Are the scenarios from the issue tested? Are edge cases covered?
- **Error handling** — What happens when things fail?

Do NOT comment on:
- Style/formatting (that's what linters are for)
- Minor naming preferences
- Import ordering

## Step 4: Produce the review

Structure the review summary as:

### Issue alignment
> Issue #N: "[issue title]"

**Verdict**: TIGHT FIX / HAS SCOPE CREEP / INCOMPLETE FIX / NO LINKED ISSUE

### Scope analysis
List every changed file with its classification (on-scope / supporting / scope creep / unrelated). For scope creep items, explain what they do and why they should be in a separate PR.

### On-scope review findings
Any bugs, security issues, or gaps in the on-scope changes.

### Completeness check
Does the PR fully address the issue? Call out any acceptance criteria or scenarios from the issue that aren't covered.

### Recommendation
- **SHIP IT** — tight fix, addresses the issue fully, no scope creep
- **TRIM AND SHIP** — the fix is good but remove the scope creep, then merge
- **NEEDS FIXES** — on-scope code has bugs or security issues
- **INCOMPLETE** — doesn't fully address the issue
- **SPLIT THIS PR** — too much going on, break it up

For each finding, post an inline review comment at the exact line using:
`gh api repos/{owner}/{repo}/pulls/{number}/comments -f body="comment" -f path="file" -F line=N -f commit_id="sha" -f side="RIGHT"`

After posting inline comments, submit an overall review using:
`gh pr review $ARGUMENTS --comment --body "summary"`

Match the review action to the verdict:
- SHIP IT → `--approve`
- TRIM AND SHIP → `--comment` (non-blocking, but call out what to remove)
- NEEDS FIXES / INCOMPLETE → `--request-changes`
- SPLIT THIS PR → `--request-changes`

Show me the full review before submitting. Wait for my go-ahead.

$ARGUMENTS
