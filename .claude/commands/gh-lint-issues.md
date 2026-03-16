Audit the quality of GitHub issues in this repo and enforce standards.

Run `gh issue list --state open --limit 100 --json number,title,body,labels,comments,createdAt,updatedAt,author` to pull the backlog.

Check each issue against these quality gates:

### Title quality
- [ ] Specific and descriptive (not "bug", "fix this", "error")
- [ ] Indicates the component or area affected
- Flag issues with vague titles and suggest improvements

### Body quality
- [ ] Has a clear problem statement
- [ ] Has reproduction steps (for bugs) or acceptance criteria (for features)
- [ ] References relevant files, endpoints, or components
- Flag empty or single-line issue bodies

### Labels
- [ ] Has at least one label
- Flag unlabelled issues and suggest labels from `gh label list`

### Comments
- [ ] Issues older than 7 days have at least one comment (acknowledgement, clarification request, or status update)
- Flag silent issues

### Staleness
- [ ] No activity in 30+ days = stale
- [ ] No activity in 90+ days = candidate for closing
- Flag both categories

Output a report card:

```
ISSUE QUALITY REPORT
====================
Total open: N
Passing all checks: N (X%)
Needs attention: N

FAILING ISSUES:
#123 - "vague title" — missing: labels, repro steps, comments
#456 - "stale" — no activity since YYYY-MM-DD
...
```

Then ask which issues I want to fix. For each, you can:
- Post a comment asking for more detail via `gh issue comment`
- Add labels via `gh issue edit --add-label`
- Close stale issues via `gh issue close` with a comment explaining why

$ARGUMENTS
