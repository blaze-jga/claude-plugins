---
description: Implement TypeScript code changes with engineer, tester, and reviewer workflow
---

We now need to implement the following:

$ARGUMENTS

Use the following workflow:
1. Use ts-engineer to implement the changes
2. Use ts-tester to add/update tests as necessary
3. Use ts-comment-reviewer to ensure comments meet standards
4. Use ts-code-reviewer to review the changes

If any issues are found by the sub-agents, provide the feedback back to the appropriate sub-agent to address the feedback. For example:

- If the ts-tester finds a bug or suggests code changes to improve testability, use the ts-engineer to address the feedback
- If the ts-code-reviewer identifies an issue with the tests, use the ts-tester to fix the tests

If you invoke a sub-agent after receiving feedback, **always** invoke all of the sub-agents after it in the order provided. For example, if ts-code-reviewer suggests changes for ts-engineer, you must invoke ts-tester, ts-comment-reviewer, and ts-code-reviewer.

**IMPORTANT:** Keep your changes focused on the above task. Limit any improvements or cleanup unrelated to the task.