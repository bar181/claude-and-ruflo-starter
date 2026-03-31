# Tip 2: Explore First, Then Plan, Then Code

**Separate research and planning from implementation to avoid solving the wrong problem.**

Letting Claude jump straight to coding often produces code that solves the wrong problem. Use Plan Mode to separate exploration from execution.

## The Four-Phase Workflow

### Phase 1: Explore (Plan Mode)
Enter Plan Mode (`Shift+Tab` twice). Claude reads files and answers questions without making changes.

```
read /src/auth and understand how we handle sessions and login.
also look at how we manage environment variables for secrets.
```

### Phase 2: Plan (Plan Mode)
Ask Claude to create a detailed implementation plan.

```
I want to add Google OAuth. What files need to change?
What's the session flow? Create a plan.
```

Press `Ctrl+G` to open the plan in your text editor for direct editing before Claude proceeds.

### Phase 3: Implement (Normal Mode)
Switch back to Normal Mode and let Claude code, verifying against its plan.

```
implement the OAuth flow from your plan. write tests for the
callback handler, run the test suite and fix any failures.
```

### Phase 4: Commit
Ask Claude to commit with a descriptive message and create a PR.

```
commit with a descriptive message and open a PR
```

## When to Skip Planning

Planning adds overhead. Skip it when:
- The scope is clear and the fix is small (fixing a typo, adding a log line, renaming a variable)
- You could describe the diff in one sentence

Planning is most useful when:
- You're uncertain about the approach
- The change modifies multiple files
- You're unfamiliar with the code being modified
