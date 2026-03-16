# Claude Code Project Guide

## How we use Claude Code

This repo is a reference setup for the 7Analytics team. Copy the `.claude/` folder into your own projects to get shared skills and conventions.

## Conventions

- Every PR must link to an issue. No orphan PRs.
- PRs should be tight — fix what the issue describes, nothing more. Scope creep goes in a separate PR.
- Tests are required for bug fixes (prove the bug existed, prove the fix works).
- Commit messages explain **why**, not what. The diff shows the what.
- No secrets in code. Use environment variables.

## Skills available

Type `/` in Claude Code to see all available skills.

### Code quality
- `/review` — review staged changes for bugs, security, and performance
- `/refactor` — analyse code and suggest improvements (asks before changing)
- `/test-this <file>` — generate real tests with edge cases, then run them
- `/security-audit` — OWASP-style security scan
- `/debug <error>` — systematic root cause analysis

### GitHub workflow
- `/bug-hunt` — scan codebase for real bugs
- `/gh-issue-create` — create well-structured GitHub issues
- `/gh-issue-triage` — audit and organise open issues
- `/gh-lint-issues` — quality gate for issue standards
- `/gh-fix-issue <number>` — pick up an issue and implement a fix
- `/gh-pr-review <number>` — review a PR against its linked issue, flag scope creep
- `/gh-pr-create` — create a PR with proper description
- `/gh-pipeline` — full chain: hunt → issue → fix → review → PR

### Understanding code
- `/explain` — full codebase architecture breakdown
- `/pr-summary` — generate PR description from branch diff
- `/migrate-check` — review DB migrations for safety
