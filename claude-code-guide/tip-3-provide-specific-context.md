# Tip 3: Provide Specific Context in Your Prompts

**The more precise your instructions, the fewer corrections you'll need.**

Claude can infer intent, but it can't read your mind. Reference specific files, mention constraints, and point to example patterns.

## Strategies

### Scope the task
| Before | After |
|--------|-------|
| *"add tests for foo.py"* | *"write a test for foo.py covering the edge case where the user is logged out. avoid mocks."* |

### Point to sources
| Before | After |
|--------|-------|
| *"why does ExecutionFactory have such a weird api?"* | *"look through ExecutionFactory's git history and summarize how its api came to be"* |

### Reference existing patterns
| Before | After |
|--------|-------|
| *"add a calendar widget"* | *"look at how existing widgets are implemented on the home page. HotDogWidget.php is a good example. Follow the pattern to implement a new calendar widget."* |

### Describe symptoms clearly
| Before | After |
|--------|-------|
| *"fix the login bug"* | *"users report that login fails after session timeout. check the auth flow in src/auth/, especially token refresh. write a failing test that reproduces the issue, then fix it"* |

## Ways to Provide Rich Content

- **`@` file references** -- `@src/utils/auth.js` includes the file contents directly
- **Paste images** -- drag and drop or `Ctrl+V` for screenshots, mockups, error dialogs
- **Give URLs** -- documentation and API references (use `/permissions` to allowlist domains)
- **Pipe data** -- `cat error.log | claude` sends file contents directly
- **Let Claude fetch** -- tell Claude to pull context itself using Bash, MCP tools, or file reads

## When Vague is Fine

Vague prompts can be useful for exploration: *"what would you improve in this file?"* can surface things you wouldn't have thought to ask about. Use precision when you know what you want; use openness when you're exploring.
