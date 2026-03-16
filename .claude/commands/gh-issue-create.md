Create a well-structured GitHub issue. Use `gh issue create` to post it.

If I provide a description, use that. If I say "from bug hunt" or reference previous findings, use those findings.

Before creating, construct the issue with this structure:

**Title**: Clear, specific, actionable (not "fix bug" — instead "API returns 500 when user email contains unicode")

**Body**:
```
## Problem
What's broken or missing? Include specific behaviour observed vs expected.

## Reproduction
Step-by-step or code snippet to trigger the issue. Include relevant file paths.

## Impact
Who is affected? What breaks? Is there a workaround?

## Suggested approach
Brief technical direction for whoever picks this up. Reference specific files/functions.

## Acceptance criteria
- [ ] Concrete, testable checkboxes
- [ ] Include edge cases that must be covered
```

**Labels**: Apply appropriate labels using `gh issue create --label`. Choose from existing labels in the repo (check with `gh label list` first).

Show me the full issue before posting. Wait for my confirmation, then run `gh issue create`.

$ARGUMENTS
