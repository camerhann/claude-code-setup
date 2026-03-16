Analyse the file or module I'm pointing at and suggest concrete refactoring improvements.

Focus on:
1. **Readability** — confusing names, deeply nested logic, functions doing too many things
2. **Duplication** — repeated code that should be extracted
3. **Complexity** — functions that are too long or have too many branches (suggest splitting)
4. **Type safety** — missing types, overly broad `any` types, unsafe casts
5. **Testability** — tightly coupled code that's hard to test in isolation

For each suggestion:
- Explain what's wrong and why it matters
- Show the refactored code

Then ask me which refactorings I want to apply. Don't change anything until I confirm.

$ARGUMENTS
