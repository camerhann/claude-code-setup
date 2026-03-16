Review the staged changes (`git diff --cached`). If nothing is staged, review unstaged changes (`git diff`).

Analyse for:

1. **Security** — injection, auth bypass, hardcoded secrets, unsafe deserialization, SSRF
2. **Bugs** — logic errors, off-by-ones, null/undefined paths, race conditions, unhandled promise rejections
3. **Performance** — N+1 queries, missing indexes, unnecessary loops, large allocations in hot paths
4. **Breaking changes** — modified public APIs, changed return types, removed exports
5. **Missing edge cases** — empty inputs, boundary values, concurrent access, network failures

For each finding:
- Cite the exact `file:line`
- Explain **why** it's a problem (not just what)
- Suggest a concrete fix

Skip style nits, formatting, and nitpicks. Focus only on things that could cause incidents or bugs in production.

End with a one-line overall verdict: SHIP IT, NEEDS FIXES, or HOLD FOR DISCUSSION.
