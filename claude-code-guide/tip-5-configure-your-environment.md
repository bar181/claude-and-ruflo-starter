# Tip 5: Configure Your Environment for Maximum Effectiveness

**A few setup steps make Claude Code significantly more effective across all sessions.**

## Write an Effective CLAUDE.md

Run `/init` to generate a starter file, then refine over time. Include only things that apply broadly and that Claude can't infer from code alone.

```markdown
# Code style
- Use ES modules (import/export) syntax, not CommonJS (require)
- Destructure imports when possible

# Workflow
- Be sure to typecheck when you're done making a series of code changes
- Prefer running single tests, not the whole test suite, for performance
```

### What to include vs. exclude

| Include | Exclude |
|---------|---------|
| Bash commands Claude can't guess | Anything Claude can figure out by reading code |
| Code style rules that differ from defaults | Standard conventions Claude already knows |
| Testing instructions and preferred test runners | Detailed API documentation (link instead) |
| Repository etiquette (branch naming, PR conventions) | Information that changes frequently |
| Architectural decisions specific to your project | Long explanations or tutorials |
| Common gotchas or non-obvious behaviors | File-by-file descriptions of the codebase |

**Key rule**: For each line, ask *"Would removing this cause Claude to make mistakes?"* If not, cut it. Bloated CLAUDE.md files cause Claude to ignore your actual instructions.

CLAUDE.md files can import other files: `@docs/git-instructions.md`

## Set Up Permissions

- **Auto mode** -- classifier reviews commands, blocks risky ones
- **Allowlists** -- permit specific safe tools (`npm run lint`, `git commit`)
- **Sandboxing** -- OS-level isolation for filesystem and network access

## Install CLI Tools

Tell Claude to use tools like `gh`, `aws`, `gcloud`, `sentry-cli`. The `gh` CLI is especially valuable -- Claude uses it for issues, PRs, and comments. Without it, unauthenticated GitHub API requests hit rate limits.

Claude can learn unfamiliar CLIs: *"Use 'foo-cli-tool --help' to learn about foo tool, then use it to solve A, B, C."*

## Connect MCP Servers

Run `claude mcp add` to connect external tools (Notion, Figma, databases, etc.).

## Create Custom Skills

Add `SKILL.md` files in `.claude/skills/` for domain knowledge and reusable workflows:

```markdown
# .claude/skills/fix-issue/SKILL.md
---
name: fix-issue
description: Fix a GitHub issue
disable-model-invocation: true
---
Analyze and fix the GitHub issue: $ARGUMENTS.
1. Use `gh issue view` to get the issue details
2. Search the codebase for relevant files
3. Implement the fix
4. Write and run tests
5. Create a descriptive commit and PR
```

Invoke with `/fix-issue 1234`.

## Set Up Hooks

Unlike CLAUDE.md instructions (advisory), hooks are deterministic and guaranteed:

```json
// .claude/settings.json -- auto-format after every edit
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{ "type": "command", "command": "prettier --write $CC_FILE_PATH" }]
    }]
  }
}
```

Ask Claude to write hooks for you: *"Write a hook that runs eslint after every file edit."*
