# Tip 1: Give Claude a Way to Verify Its Work

**This is the single highest-leverage thing you can do.**

Claude performs dramatically better when it can check its own output -- run tests, compare screenshots, validate results. Without verification, you become the only feedback loop, and every mistake requires your attention.

## How to Apply This

### Provide test cases upfront
Instead of: *"implement a function that validates email addresses"*

Say: *"write a validateEmail function. Test cases: user@example.com is true, 'invalid' is false, user@.com is false. Run the tests after implementing."*

### Verify UI changes visually
Instead of: *"make the dashboard look better"*

Say: *"[paste screenshot] implement this design. Take a screenshot of the result and compare it to the original. List differences and fix them."*

### Address root causes
Instead of: *"the build is failing"*

Say: *"the build fails with this error: [paste error]. Fix it and verify the build succeeds. Address the root cause, don't suppress the error."*

## Verification Methods

- **Test suites** -- tell Claude to write and run tests
- **Linters / type checkers** -- have Claude run them after changes
- **Bash commands** -- any command that checks output works
- **Screenshots** -- use the Chrome extension for visual verification
- **Expected outputs** -- provide sample inputs and expected results

Invest in making your verification rock-solid. The better Claude can self-check, the less you need to intervene.
