---
name: gh-fix-issue
description: Pick up a GitHub issue, plan the fix, implement it, and run tests
---

Pick up a GitHub issue and implement a fix for it.

Steps:

### 1. Understand the issue
Run `gh issue view $ARGUMENTS` to read the full issue including comments. If the issue lacks detail, tell me what's missing before proceeding.

### 2. Investigate
- Find the relevant code paths mentioned in the issue
- Read the surrounding code to understand context
- Check git blame if needed to understand why the code is the way it is
- Look at related tests to understand expected behaviour

### 3. Plan the fix
Before writing code, briefly explain:
- Root cause
- Proposed fix
- Files that will change
- Any risks or tradeoffs

Wait for my approval before implementing.

### 4. Implement
- Create a feature branch: `git checkout -b fix/issue-N-short-description`
- Make the minimum changes needed to fix the issue
- Follow existing code patterns and conventions in the repo
- Add or update tests that verify the fix
- Run existing tests to check nothing is broken

### 5. Verify
- Run the test suite
- If tests fail, fix them
- Show me a summary of all changes made

### 6. Ship it
- Review the changes for obvious issues (security, breaking changes, missing tests)
- Commit with a clear message referencing the issue: `Fixes #N: short description`
- Ask: **"Ready for me to create a PR for this?"**
- If yes, create a PR using `gh pr create` with `Closes #N` in the body, a clear summary of the fix, and what was tested

$ARGUMENTS
