# Tip 4: Manage Context Aggressively

**Context is your most important resource. When it fills up, Claude's performance degrades.**

Claude's context window holds your entire conversation -- every message, file read, and command output. It fills up fast. A single debugging session might consume tens of thousands of tokens. As context fills, Claude starts "forgetting" earlier instructions or making more mistakes.

## Key Actions

### Use `/clear` between tasks
Reset the context window entirely when switching between unrelated tasks. This is the simplest and most effective thing you can do.

### Use subagents for research
Since research reads lots of files (all consuming context), delegate it to subagents that run in separate context windows:

```
Use subagents to investigate how our authentication system handles token
refresh, and whether we have any existing OAuth utilities I should reuse.
```

The subagent explores, reads files, and reports back a summary -- without cluttering your main conversation.

### Compact when needed
- Auto-compaction triggers automatically near limits, preserving code and key decisions
- Run `/compact <instructions>` for manual control, e.g., `/compact Focus on the API changes`
- Use `/rewind` to summarize from a specific point forward

### Use `/btw` for quick questions
Side questions via `/btw` appear in a dismissible overlay and never enter conversation history.

### Course-correct early
- **`Esc`** -- stop mid-action, context preserved, redirect
- **`Esc + Esc`** -- rewind to previous checkpoint
- **`/clear` after 2+ failed corrections** -- a clean session with a better prompt almost always outperforms accumulated corrections

## Common Anti-Patterns

| Problem | Fix |
|---------|-----|
| **Kitchen sink session** -- mixing unrelated tasks | `/clear` between tasks |
| **Correction spiral** -- correcting the same issue repeatedly | After 2 failures, `/clear` and write a better initial prompt |
| **Infinite exploration** -- unscoped "investigate" prompts | Scope narrowly or use subagents |

## Resume Without Losing Context

```bash
claude --continue    # Resume most recent conversation
claude --resume      # Pick from recent sessions
```

Use `/rename` to give sessions descriptive names like `"oauth-migration"` so you can find them later.
