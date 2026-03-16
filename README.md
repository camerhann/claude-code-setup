# Claude Code Team Setup

Reference configuration for using Claude Code at 7Analytics. Clone this, copy what you need into your projects.

## Quick start

1. Install Claude Code: `npm install -g @anthropic-ai/claude-code`
2. Copy the `.claude/` folder into your project root
3. Commit it — the whole team gets the skills via git

```bash
cp -r claude-code-setup/.claude/ your-project/.claude/
cd your-project
git add .claude/
git commit -m "Add Claude Code team skills"
```

## What's included

### `.claude/commands/` — Team skills

These become `/project:` commands available to everyone who clones the repo. Type `/project:` in Claude Code to see them all.

**Everyday workflow:**
- `/project:review` — review your changes before committing
- `/project:test-this src/file.ts` — generate tests for a file
- `/project:debug "error message"` — systematic debugging
- `/project:explain` — understand an unfamiliar codebase

**GitHub integration (requires `gh` CLI):**
- `/project:gh-fix-issue 42` — read issue #42, plan, implement, test
- `/project:gh-pr-review 15` — review PR #15 against its linked issue
- `/project:gh-pr-create` — create a PR from your current branch
- `/project:gh-lint-issues` — audit issue quality across the repo
- `/project:gh-pipeline` — full bug-to-PR pipeline with approval gates

### `CLAUDE.md` — Project conventions

Tells Claude Code how your team works. Edit this per-project to include:
- Coding conventions and patterns
- Build/test commands
- Architecture decisions
- Anything you'd tell a new team member

## Personal commands

If you want commands available in ALL your projects (not just one repo), put `.md` files in `~/.claude/commands/`. These show up as `/user:` commands.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated (for GitHub skills)
- Claude Code Teams subscription
