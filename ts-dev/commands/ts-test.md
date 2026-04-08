---
description: Update TypeScript tests with tester and reviewer workflow
---

Update the tests for the following changes:

$ARGUMENTS

Use the following workflow:
1. Use ts-tester to add/update tests as necessary
2. Use ts-comment-reviewer to ensure comments meet standards
3. Use ts-code-reviewer to review the changes

If ts-tester suggests changes to the code, use ts-engineer to implement the changes.

If any issues are found by the sub-agents, provide the feedback back to the appropriate sub-agent to address the feedback. For example:

- If the ts-tester finds a bug or suggests code changes to improve testability, use the ts-engineer to address the feedback
- If the ts-code-reviewer identifies an issue with the tests, use the ts-tester to fix the tests

If you invoke a sub-agent after receiving feedback, **always** invoke all of the sub-agents after it in the order provided. For example, if ts-code-reviewer suggests changes for ts-tester, you must invoke ts-tester, ts-comment-reviewer, and ts-code-reviewer.

**IMPORTANT:** Keep your changes focused on the above task. Limit any improvements or cleanup unrelated to the task.