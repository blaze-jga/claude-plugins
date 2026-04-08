---
description: Implement Playwright tests with planner, AQA agent, and reviewer workflow
---

We now need to implement the following:

$ARGUMENTS

**If the task is unclear or ambiguous, ask clarifying questions before proceeding with implementation.**

Use the following workflow:

1. Use playwright-test-planner to analyze the requirements and create a comprehensive test plan (plan.md)
2. Use playwright-aqa-agent to implement the tests based on the plan created by the planner
3. Use playwright-aqa-reviewer-agent to review the implemented test changes

If any issues are found by the sub-agents, provide the feedback back to the appropriate sub-agent to address the feedback. For example:

- If the playwright-test-planner creates an unclear or incomplete plan, ask clarifying questions or request the planner to revise specific sections
- If the playwright-aqa-reviewer-agent identifies issues with the tests (flakiness, poor selectors, code duplication, etc.), use the playwright-aqa-agent to fix the tests
- If the playwright-aqa-agent discovers bugs in the application code or suggests changes to improve testability, report these findings to the user
- If implementation reveals that the plan was unrealistic or missing scenarios, provide feedback to playwright-test-planner to update the plan, then have playwright-aqa-agent implement the revised plan

If you invoke a sub-agent after receiving feedback, always invoke all of the sub-agents after it in the order provided. For example:
- If playwright-test-planner revises the plan, invoke playwright-aqa-agent and then playwright-aqa-reviewer-agent
- If playwright-aqa-reviewer-agent suggests changes for playwright-aqa-agent, invoke playwright-aqa-agent and then playwright-aqa-reviewer-agent again

**IMPORTANT:** Keep your changes focused on the above task. Limit any improvements or cleanup unrelated to the task.
