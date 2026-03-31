# Claude Code Overview

*Source: [Official Claude Code Docs](https://code.claude.com/docs/en/overview)*

## What is Claude Code?

Claude Code is an agentic coding tool by Anthropic that reads your codebase, edits files, runs commands, and integrates with your development tools. Unlike a chatbot that answers questions and waits, Claude Code can autonomously explore, plan, and implement solutions while you watch, redirect, or step away entirely.

It is available across multiple environments:

- **Terminal CLI** -- full-featured command-line interface (`curl -fsSL https://claude.ai/install.sh | bash`, then `cd your-project && claude`)
- **VS Code / Cursor** -- extension with inline diffs, @-mentions, plan review, and conversation history
- **JetBrains IDEs** -- plugin for IntelliJ, PyCharm, WebStorm, etc.
- **Desktop App** -- standalone app for Mac and Windows with visual diff review and multi-session support
- **Web** -- browser-based at [claude.ai/code](https://claude.ai/code) with no local setup required
- **iOS App** -- kick off tasks from your phone

All surfaces connect to the same underlying engine, so your CLAUDE.md files, settings, and MCP servers work across all of them.

## Core Capabilities

### Code Generation & Editing
Describe what you want in plain language. Claude Code plans the approach, writes code across multiple files, and verifies it works. For bugs, paste an error message or describe the symptom -- it traces the issue, identifies the root cause, and implements a fix.

### Git Integration
Works directly with git: stages changes, writes commit messages, creates branches, and opens pull requests. Example: `claude "commit my changes with a descriptive message"`

### Automation & Scripting
Follows the Unix philosophy -- pipe logs into it, run it in CI, or chain it with other tools:
```bash
tail -200 app.log | claude -p "Slack me if you see any anomalies"
git diff main --name-only | claude -p "review these changed files for security issues"
```

### MCP (Model Context Protocol)
Connect external tools via the open MCP standard -- read design docs from Google Drive, update Jira tickets, pull data from Slack, query databases, or integrate Figma designs.

### CLAUDE.md Project Instructions
A markdown file at your project root that Claude reads at the start of every session. Use it for coding standards, architecture decisions, preferred libraries, and workflow rules. Claude also builds auto-memory as it works, saving build commands and debugging insights across sessions.

### Custom Skills & Hooks
- **Skills**: Reusable workflows in `.claude/skills/` (e.g., `/fix-issue 1234`)
- **Hooks**: Deterministic shell commands that run before/after Claude actions (e.g., auto-format after every edit, lint before commit)
- **Custom Subagents**: Specialized agents in `.claude/agents/` with their own tools and context

### Parallel Sessions & Agent Teams
Spawn multiple agents working on different parts of a task simultaneously. A lead agent coordinates work, assigns subtasks, and merges results. Run sessions in isolated git worktrees to prevent conflicts.

### Scheduled Tasks
Run Claude on recurring schedules -- morning PR reviews, overnight CI failure analysis, weekly dependency audits. Available via cloud (runs even when your computer is off), desktop app, GitHub Actions, or `/loop` for in-session polling.

## Key Modes

| Mode | Purpose |
|------|---------|
| **Normal Mode** | Full read/write access -- Claude explores, edits, and runs commands |
| **Plan Mode** (`Shift+Tab` or `--permission-mode plan`) | Read-only analysis -- Claude explores and plans without making changes |
| **Auto Mode** (`--permission-mode auto`) | A classifier handles permission approvals, blocking only risky actions |

## Session Management

- **`/clear`** -- reset context between unrelated tasks
- **`Esc`** -- stop Claude mid-action (context preserved)
- **`Esc + Esc` or `/rewind`** -- restore conversation and code state to any checkpoint
- **`claude --continue`** -- resume most recent conversation
- **`claude --resume`** -- pick from recent sessions
- **`/compact`** -- manually compress context with optional focus instructions

## Non-Interactive (Headless) Mode

Run Claude without a session for CI/CD, scripts, and pipelines:
```bash
claude -p "Explain what this project does"                    # plain text
claude -p "List all API endpoints" --output-format json       # structured JSON
claude -p "Analyze this log file" --output-format stream-json # streaming
```

## The Most Important Constraint

**Claude's context window fills up fast, and performance degrades as it fills.** Every message, file read, and command output consumes context. Managing this is the single most important factor in getting good results. Use `/clear` between tasks, delegate research to subagents, and keep CLAUDE.md concise.
