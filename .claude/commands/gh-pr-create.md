Create a pull request for the current branch and push it to GitHub.

Steps:
1. Determine base branch (`main` or `master`)
2. Check current branch: `git branch --show-current` — refuse if on main/master
3. Check for uncommitted changes — warn me if there are any
4. Read all commits since branching: `git log base..HEAD --oneline`
5. Read the full diff: `git diff base...HEAD`
6. Read changed files for context where the diff alone isn't clear enough
7. Check if branch is pushed: `git status -sb` — push with `-u` if needed

Build the PR:

**Title**: Under 70 chars, describes the change (not "Update files")

**Body**:
```
## Summary
2-4 bullets on what changed and WHY (business impact first, then technical detail)

## Changes
Grouped by area with brief descriptions

## Testing
How this was tested. Call out manual verification needed.

## Risks
Breaking changes, migration steps, rollback plan. "Low risk" if none.

## Related issues
Link any related issues with `Closes #N` or `Related to #N`
```

**Labels & reviewers**: Check available labels (`gh label list`) and suggest appropriate ones. Ask who should review.

Show me the full PR before creating. On my confirmation, run `gh pr create` with the title, body, labels, and reviewers.

$ARGUMENTS
