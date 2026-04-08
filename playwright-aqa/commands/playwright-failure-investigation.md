---
description: Investigate and resolve Playwright test failures with analysis, planning, and resolution workflow
---

We now need to implement the following:

$ARGUMENTS

**If the task is unclear or ambiguous, ask clarifying questions before proceeding with implementation.**

Use the following workflow:

## Phase 1: Investigation & Analysis

1. **Use playwright-failure-analyst** to perform deep analysis of the test failure:
   - Analyze test code, traces, screenshots, and logs
   - Use Playwright MCP browser tools to reproduce and investigate interactively
   - Determine root cause: Test Issue vs Application Bug vs Environmental Issue
   - Create comprehensive failure analysis document (e.g., `failure-analysis-{test-name}-{date}.md`)
   - Provide evidence-based findings and recommendations

2. **Use playwright-test-planner** to review the failure analysis and plan resolution:
   - Analyze the `failure-analysis-{test-name}-{date}.md` document
   - Determine the appropriate resolution path (A, B, C, or D)
   - Create a **concise, actionable** implementation plan for the AQA agent (max 100-150 lines)
   - **IMPORTANT**: Present the DRAFT plan to the user (do NOT create document yet)
   - **Iterate with user**: Incorporate feedback and revise the plan as needed
   - Once the plan is approved by the user, create/update `implementation-plan-{test-name}-{date}.md` with the final version
   - Only proceed to Phase 2 after the document is finalized and user gives explicit confirmation

   **Plan Format Requirements** (keep it concise):
   - **Summary**: 2-3 sentences on root cause and fix approach
   - **Files to Change**: List specific files and line numbers
   - **Code Changes**: Show old vs new code snippets
   - **Testing Steps**: Brief validation steps (2-4 commands)
   - **Skip**: Detailed rationales, verbose explanations, multiple verification points, and step-by-step breakdowns

## Phase 2: Resolution Path

**After user approval of the implementation plan**, proceed with the appropriate resolution based on the playwright-test-planner's plan:

### Path A: Test Code Issue

If the root cause is a **test code issue** (flaky test, brittle selectors, race conditions, test logic error, etc.):

1. **Use playwright-aqa-agent** to fix the test based on implementation plan recommendations:
   - Implement specific fixes from the implementation plan
   - Use Playwright MCP to verify the fix works
   - Run the test 3+ times to ensure stability
   - Continue fixing until test passes consistently

2. **Use playwright-aqa-reviewer-agent** to review the fix:
   - Verify the fix is proper and doesn't introduce new issues
   - Ensure best practices are followed
   - Check for any remaining flakiness patterns

3. If reviewer finds issues, loop back to playwright-aqa-agent to address feedback, then re-review

### Path B: Application Bug

If the root cause is an **application bug**:

1. **Create Jira-ready bug report** using the following template:

```markdown
# Bug Report: [Concise Bug Title]

## Summary
[1-2 sentence description of the bug]

## Environment
- **Environment**: [QA/Staging/Production]
- **Browser**: [Chrome/Firefox/Safari + version]
- **Application Version**: [Version or commit hash if available]
- **Date Discovered**: [Date]

## Severity
[Critical | High | Medium | Low]

## Priority
[P0 | P1 | P2 | P3]

## Description
[Detailed description of what is wrong]

## Steps to Reproduce
1. [Step 1]
2. [Step 2]
3. [Step 3]
...

## Expected Behavior
[What should happen]

## Actual Behavior
[What actually happens]

## Evidence
### Screenshots
![Screenshot description](path or attach file)

### Console Errors
[JavaScript console errors if applicable]

### Network Issues
[Failed API calls, error responses, etc.]

### Video/Trace
[Link to Playwright trace or video if available]

## Impact
- **User Impact**: [How this affects end users]
- **Affected Functionality**: [What features are broken]
- **Workaround**: [If any workaround exists]

## Additional Context
- **Test File**: [Path to failing test]
- **Failure Analysis**: [Link to detailed failure analysis document]
- **Related Issues**: [Links to related bugs or tickets]

## Technical Details
[Stack traces, error messages, relevant code sections]

## Suggested Fix
[If obvious, suggest potential solution or area to investigate]
```

2. Save the bug report as `bug-report-{summary}-{date}.md`

3. Provide the user with:
   - The bug report file ready to copy into Jira
   - Summary of the bug and its impact
   - Recommendation on whether to skip/disable the test until bug is fixed

### Path C: Environmental Issue

If the root cause is an **environmental issue** (infrastructure, timing, resource constraints):

1. Document the environmental issue in the failure analysis
2. Provide recommendations for:
   - Configuration changes needed
   - Infrastructure adjustments required
   - CI/CD pipeline modifications
3. If possible, make test more resilient to environmental variations using playwright-aqa-agent

### Path D: Test Data Issue

If the root cause is a **test data issue**:

1. **Use playwright-aqa-agent** to:
   - Fix test data setup/teardown
   - Implement proper test isolation
   - Add data validation and error handling
   - Ensure test data is properly managed

## Phase 3: Validation & Documentation

1. **Ensure all artifacts are created**:
   - Failure analysis document exists
   - Implementation plan document exists
   - Bug report exists (if application bug)
   - Fixed test passes consistently (if test issue)
   - All recommendations are documented

2. **Summary for user**:
   - Root cause determination
   - Actions taken
   - Files created/modified
   - Next steps required

## Feedback Loop

If any sub-agent identifies additional issues during their work:

- If playwright-aqa-agent discovers the issue is actually a product bug while fixing, loop back to create bug report
- If playwright-aqa-reviewer-agent finds the fix insufficient, loop back to playwright-aqa-agent
- If new failure patterns emerge during testing, loop back to playwright-failure-analyst for additional investigation

## Important Notes

- **User approval required**: Always wait for user approval of the implementation plan before proceeding to Phase 2
- **Be thorough**: Investigation must be comprehensive with clear evidence
- **Use MCP tools**: Always use Playwright MCP browser tools for hands-on investigation
- **Close browsers**: Always close the browser after using Playwright MCP tools - do not leave browsers open
- **Document everything**: Create clear, detailed documents for all findings
- **Test fixes properly**: Run tests multiple times only for flaky test fixes (3+ runs), once for regular fixes
- **Run only affected specs**: Never run entire test suite - identify and run only affected test spec files
- **Prioritize correctly**: Critical bugs should be flagged immediately
- **Stay focused**: Only investigate and fix the reported failure unless asked to do broader analysis

## Example Workflows

### Example 1: Flaky Test
```
User provides failing test → 
playwright-failure-analyst investigates → 
Determines: Race condition in test code → 
playwright-test-planner creates implementation plan → 
User approves plan → 
playwright-aqa-agent fixes with proper waits → 
playwright-aqa-reviewer-agent reviews → 
Test runs 3x successfully → 
Done
```

### Example 2: Application Bug
```
User provides failing test → 
playwright-failure-analyst investigates → 
Determines: API returning 500 error → 
playwright-test-planner creates implementation plan → 
User approves plan → 
Create Jira-ready bug report → 
Recommend skipping test until bug fixed → 
Done
```

### Example 3: Mixed Issue
```
User provides failing test → 
playwright-failure-analyst investigates → 
Determines: App has bug BUT test is also poorly written → 
playwright-test-planner creates implementation plan → 
User approves plan → 
Create bug report for app issue → 
playwright-aqa-agent improves test code → 
Test properly catches the bug now → 
Done
```

**IMPORTANT:** Keep investigation focused on the specific failure provided. If you discover broader issues, mention them but don't automatically expand scope unless user requests it.
