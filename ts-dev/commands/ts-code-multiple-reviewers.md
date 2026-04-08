---
description: Implement TypeScript code changes with engineer, tester, and parallel reviewer workflow (3x code reviewers + 3x requirement reviewers)
---

We now need to implement the following:

$ARGUMENTS

Use the following workflow:
1. Use ts-engineer to implement the changes
2. Use ts-tester to add/update tests as necessary
3. Use ts-comment-reviewer to ensure comments meet standards
4. Run the review phase: spawn all 6 reviewers **simultaneously in parallel**:
   - 3 independent instances of ts-code-reviewer
   - 3 independent instances of ts-requirement-reviewer
5. Collect all feedback from the 6 reviewers, deduplicate overlapping findings, then consolidate into a single unified feedback report before proceeding

If any issues are found by the sub-agents, provide the consolidated feedback back to the appropriate sub-agent to address it. For example:

- If a ts-tester finds a bug or suggests code changes to improve testability, use ts-engineer to address the feedback
- If a ts-code-reviewer identifies an issue with the tests, use ts-tester to fix the tests
- If a ts-requirement-reviewer identifies a missing or incorrect requirement, use ts-engineer to address it

If you invoke a sub-agent after receiving feedback, **always** invoke all of the sub-agents after it in the order provided. For example, if a reviewer suggests changes for ts-engineer, you must invoke ts-tester, ts-comment-reviewer, and the full parallel review phase again (all 6 reviewers simultaneously).

**IMPORTANT:** Keep your changes focused on the above task. Limit any improvements or cleanup unrelated to the task.
