Perform a security audit on this codebase or the specified files.

Check for:

1. **Injection** — SQL injection, command injection, XSS, template injection, LDAP injection
2. **Authentication & Authorization** — missing auth checks, broken access control, privilege escalation paths
3. **Secrets** — hardcoded API keys, passwords, tokens, connection strings (check .env files, config, and source)
4. **Data exposure** — sensitive data in logs, error messages, API responses, or client-side code
5. **Dependencies** — known vulnerable packages (check lock files against known CVEs if possible)
6. **Cryptography** — weak algorithms, insufficient key lengths, improper IV/nonce usage
7. **Input validation** — missing or insufficient validation at system boundaries
8. **SSRF / Open redirects** — unvalidated URLs, user-controlled redirect targets

For each finding:
- **Severity**: CRITICAL / HIGH / MEDIUM / LOW
- **Location**: `file:line`
- **Description**: What the vulnerability is and how it could be exploited
- **Fix**: Concrete code change to remediate

$ARGUMENTS
