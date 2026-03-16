---
name: bug-hunt
description: Deep codebase scan for real bugs — not style issues, production incident risks
---

Scan this codebase for bugs. Not style issues — real bugs that could cause production incidents.

Methodology:
1. Read the project structure and understand what this codebase does
2. Focus on high-risk areas: error handling, data validation, auth, concurrency, state management, API boundaries
3. Trace data flows end-to-end looking for places where assumptions break

Look for:
- Unhandled error paths (uncaught exceptions, missing error callbacks, swallowed errors)
- Race conditions and concurrency bugs
- Null/undefined access without guards
- Off-by-one errors in loops or pagination
- SQL injection or other injection vulnerabilities
- Type coercion bugs (especially in JS/TS/Python)
- Resource leaks (unclosed connections, file handles, event listeners)
- Logic errors in business rules
- API contract mismatches (frontend expects X, backend returns Y)

Output each finding as:

### BUG-001: [Short title]
- **Severity**: CRITICAL / HIGH / MEDIUM
- **File**: `file:line`
- **Description**: What's wrong and how it manifests
- **Reproduction**: How this bug would trigger in practice
- **Suggested fix**: Concrete code change

At the end, ask: "Want me to create GitHub issues for any of these? Or fix them directly?"

$ARGUMENTS
