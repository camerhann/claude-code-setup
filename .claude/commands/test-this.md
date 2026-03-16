Read the file or function I'm pointing at and generate thorough tests for it.

Requirements:
- Read the actual implementation first — understand the logic, branches, and edge cases
- Use the testing framework already in the project (check package.json, pyproject.toml, Cargo.toml, etc.)
- Follow existing test patterns and conventions in the repo (check existing test files for style)

Cover these categories:
1. **Happy path** — normal expected inputs and outputs
2. **Edge cases** — empty inputs, nulls, zeros, max values, boundary conditions
3. **Error cases** — invalid inputs, network failures, missing data, permission errors
4. **Integration** — if the function calls external services or DB, test those interactions

Do NOT:
- Write trivial tests that just assert true === true
- Over-mock to the point the test doesn't verify real behaviour
- Skip testing error paths

After writing the tests, run them to make sure they pass. Fix any failures.

$ARGUMENTS
