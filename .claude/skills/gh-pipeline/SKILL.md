---
name: gh-pipeline
description: Full bug-to-PR pipeline: hunt, issue, fix, review, ship — with approval gates
---

Run the full bug-to-PR pipeline on this codebase. This chains together multiple steps — I'll confirm at each gate before proceeding.

## Phase 1: Hunt
Scan the codebase for bugs, focusing on the area or files specified. Identify real issues, not style nits.

**Gate: Show findings. Ask which ones to proceed with.**

## Phase 2: Issues
For each confirmed finding, create a well-structured GitHub issue with reproduction steps, impact assessment, and suggested approach.

**Gate: Show draft issues. Confirm before posting to GitHub.**

## Phase 3: Fix
For each issue (or the ones I select), implement the fix:
- Create a feature branch
- Write the minimum code change
- Add/update tests
- Run the test suite

**Gate: Show the diff. Ask for approval.**

## Phase 4: Review
Self-review the changes:
- Check for security issues
- Check for performance regressions
- Verify test coverage
- Check for breaking changes

**Gate: Show review findings. Fix any issues found.**

## Phase 5: Ship
- Commit with a clear message referencing the issue number
- Create a PR linking to the issue with `Closes #N`
- Request review if I specify reviewers

**Gate: Show PR draft. Confirm before creating.**

At any point I can say "stop" to exit the pipeline, or "skip" to jump to the next phase.

$ARGUMENTS
