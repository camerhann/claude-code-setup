---
name: pr-summary
description: Generate a pull request description from the current branch diff
---

Generate a pull request summary for the current branch.

Steps:
1. Determine the base branch (usually `main` or `master`)
2. Run `git log base..HEAD` to see all commits on this branch
3. Run `git diff base...HEAD` to see the full changeset
4. Read any changed files that need more context to understand the intent

Write the PR in this format:

## Summary
2-4 bullet points explaining **what** changed and **why**. Lead with the user-facing or business impact, not the technical details.

## Changes
Group changes by area (e.g., API, Frontend, Database, Tests). For each group, briefly describe what was done.

## Testing
Describe how this was tested or should be tested. Call out any areas that need manual verification.

## Risks
Note anything that could go wrong — breaking changes, migration risks, performance concerns. If none, say "Low risk."

Keep it concise. Reviewers should understand the PR in under 60 seconds.
