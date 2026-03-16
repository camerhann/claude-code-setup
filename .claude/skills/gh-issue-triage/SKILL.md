---
name: gh-issue-triage
description: Audit open GitHub issues for completeness, duplicates, and staleness
---

Triage open GitHub issues in this repo. Run `gh issue list --state open --limit 50` to get the current backlog.

For each issue, assess:

1. **Completeness** — Does it have enough detail to act on? (reproduction steps, expected behaviour, context)
2. **Labels** — Is it properly labelled? Suggest missing labels from the repo's label set (`gh label list`)
3. **Duplicates** — Does it overlap with another open issue? Flag potential duplicates
4. **Staleness** — No activity in 30+ days? Flag it
5. **Missing comments** — Issues with 0 comments that are >7 days old need attention

Output a triage report:

### Needs more detail
Issues that can't be worked on without clarification. Suggest a comment to post asking for specifics.

### Missing labels
Issues that should be categorised. Suggest labels.

### Potential duplicates
Groups of issues that might be the same problem.

### Stale
Issues with no activity that should be closed or revived.

### Ready to work
Issues that are well-defined and could be picked up now.

Ask which actions I want to take — you can post comments, add labels, and close issues via `gh` CLI on my confirmation.

$ARGUMENTS
