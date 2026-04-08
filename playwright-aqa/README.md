# Playwright Testing Agent Ecosystem

This directory contains specialized AI agents and commands for comprehensive Playwright test automation workflows.

## 📁 Directory Structure

```
.claude/
├── agents/                    # Specialized AI agents
│   ├── playwright-test-planner.md         # Test planning & strategy
│   ├── playwright-aqa-agent.md            # Test implementation & fixes
│   ├── playwright-aqa-reviewer.md         # Code quality review
│   └── playwright-failure-analyst.md      # Failure investigation & analysis
├── commands/                  # Multi-agent workflow commands
│   ├── playwright-test.md                # Plan → Implement → Review workflow
│   └── playwright-failure-investigation.md             # Complete failure investigation workflow
└── settings.local.json        # Permissions configuration
```

## 🤖 Available Agents

### 1. playwright-test-planner
**Role:** Test Architect & Strategist (Planning Only - No Coding)

**Use when:**
- Designing test strategies for new features
- Creating comprehensive test plans
- Identifying test scenarios and edge cases
- Need structured test documentation before implementation

**Output:** `plan.md` files with detailed test scenarios

**Invoke:** `@playwright-test-planner`

---

### 2. playwright-aqa-agent
**Role:** Test Implementation & Development

**Use when:**
- Writing new automated tests
- Fixing broken or flaky tests
- Refactoring test code
- Implementing tests from plans
- Need hands-on test coding work

**Key Features:**
- Uses Playwright MCP for interactive testing
- Runs tests multiple times to verify stability
- Applies best practices and patterns
- Reuses existing page objects and helpers

**Invoke:** `@playwright-aqa-agent`

---

### 3. playwright-aqa-reviewer
**Role:** Code Quality & Best Practices Review

**Use when:**
- Reviewing test code quality
- Identifying potential flakiness issues
- Checking for code duplication
- Validating test patterns and architecture
- Need expert feedback on test implementation

**Output:** Detailed feedback with specific recommendations

**Invoke:** `@playwright-aqa-reviewer`

---

### 4. playwright-failure-analyst
**Role:** Forensic Failure Investigation & Analysis

**Use when:**
- Tests are failing and you need to know why
- Determining if failure is test issue or product bug
- Investigating flaky test patterns
- Need comprehensive failure analysis
- Creating bug reports for product issues

**Key Features:**
- Uses Playwright MCP for hands-on investigation
- Analyzes traces, screenshots, logs, and network activity
- Creates detailed failure analysis documents
- Generates Jira-ready bug reports
- Differentiates between test issues vs app bugs

**Output:** 
- `failure-analysis-{test}-{date}.md` - Comprehensive investigation report
- `bug-report-{summary}-{date}.md` - Jira-ready bug documentation

**Invoke:** `@playwright-failure-analyst`

---

## 🚀 Commands (Multi-Agent Workflows)

### `/cmd playwright-test`
**Purpose:** Complete workflow for writing new tests

**Flow:**
```
Planner → Creates test plan
   ↓
AQA Agent → Implements tests
   ↓
Reviewer → Reviews quality
   ↓
(Loop if issues found)
```

**Use when:** Adding new test coverage for features

---

### `/cmd playwright-failure-investigation`
**Purpose:** Complete workflow for investigating and resolving test failures

**Flow:**
```
Failure Analyst → Investigates with MCP tools
   ↓
Determines root cause:
   ├── Test Issue → AQA Agent fixes → Reviewer validates
   ├── App Bug → Create Jira bug report
   ├── Environment → Document recommendations
   └── Data Issue → AQA Agent fixes data handling
```

**Use when:** Tests failing and need root cause analysis

**Outputs:**
- Failure analysis document
- Bug report (if product bug)
- Fixed test code (if test issue)

---

## 🎯 Quick Start Guide

### Scenario 1: "My test is failing"
```bash
Use: /cmd playwright-failure-investigation
Provide: Test name, file path, or error message
Result: Root cause analysis + fix or bug report
```

### Scenario 2: "I need to write tests for a new feature"
```bash
Use: /cmd playwright-test
Provide: Feature description or requirements
Result: Plan → Implementation → Review
```

### Scenario 3: "Is my test code good quality?"
```bash
Use: @playwright-aqa-reviewer
Provide: Test file(s) to review
Result: Detailed quality feedback
```

### Scenario 4: "I need a test plan but won't implement yet"
```bash
Use: @playwright-test-planner
Provide: Feature requirements
Result: Comprehensive plan.md
```

### Scenario 5: "Fix this specific test"
```bash
Use: @playwright-aqa-agent
Provide: Test file and what needs fixing
Result: Fixed test code
```

---

## 🔄 Typical Workflows

### Workflow: New Feature Testing
1. **Plan** - `@playwright-test-planner` creates strategy
2. **Implement** - `@playwright-aqa-agent` writes tests
3. **Review** - `@playwright-aqa-reviewer` validates quality
4. **Verify** - Run 3+ times to ensure stability

*Or use: `/cmd playwright-test` for complete workflow*

---

### Workflow: Test Failure Investigation
1. **Investigate** - `@playwright-failure-analyst` does forensics
2. **Categorize** - Determines if test issue or app bug
3. **Resolve**:
   - **If test issue**: `@playwright-aqa-agent` fixes
   - **If app bug**: Create bug report for Jira
   - **If environment**: Document infrastructure needs
4. **Validate** - Verify fix with multiple runs

*Or use: `/cmd playwright-failure-investigation` for complete workflow*

---

### Workflow: Flaky Test Resolution
1. **Analyze** - `@playwright-failure-analyst` identifies patterns
2. **Fix** - `@playwright-aqa-agent` implements proper synchronization
3. **Review** - `@playwright-aqa-reviewer` validates approach
4. **Verify** - Run 3+ times to confirm stability

---

## ⚙️ Prerequisites: Playwright MCP Setup in Claude Desktop

To use the agents that interact with the browser (playwright-aqa-agent, playwright-failure-analyst), you need to pre-install the Playwright MCP server in Claude Desktop.
`claude mcp add playwright npx @playwright/mcp@latest`
More info: https://github.com/microsoft/playwright-mcp

---

## 📊 Failure Analysis Output

When using the failure analyst, you'll get documents like:

### `failure-analysis-{test}-{date}.md`
```markdown
# Test Failure Analysis
- Executive Summary
- Investigation Process  
- Root Cause Determination
- Evidence (screenshots, logs, traces)
- Impact Assessment
- Specific Recommendations
- Follow-up Actions
```

### `bug-report-{summary}-{date}.md` (if product bug)
```markdown
# Bug Report (Jira-Ready)
- Summary & Severity
- Steps to Reproduce
- Expected vs Actual Behavior
- Evidence & Screenshots
- Impact Analysis
- Technical Details
```

---

## 💡 Best Practices

### DO:
✅ Use `/cmd playwright-failure-investigation` when tests fail  
✅ Let failure analyst determine root cause before fixing  
✅ Create bug reports for product issues  
✅ Run tests multiple times after fixes  
✅ Use MCP tools for hands-on investigation  
✅ Review test code quality before merging  
✅ Plan complex features before implementing tests  

### DON'T:
❌ Skip failure investigation and jump to fixes  
❌ Assume flakiness without investigation  
❌ Let product bugs go undocumented  
❌ Merge tests that haven't been run multiple times  
❌ Ignore reviewer feedback  
❌ Over-complicate simple tasks with full workflows  

---

## 📖 Documentation

- **Agent Details**: See individual agent files in `agents/` directory
- **Command Details**: See individual command files in `commands/` directory

---

## 🆘 Troubleshooting

**Q: When should I use a command vs single agent?**  
A: Commands for multi-step workflows (new tests, failure investigation). Single agents for focused tasks.

**Q: Test is flaky, what do I do?**  
A: Use `/cmd playwright-failure-investigation` - it will analyze the pattern and guide proper fix.

**Q: Need to create a bug report?**  
A: Use `/cmd playwright-failure-investigation` - it will create Jira-ready bug reports automatically.

**Q: Agent isn't using MCP tools?**  
A: Explicitly mention "use Playwright MCP browser tools" or "investigate interactively".

**Q: How do I know if it's a test bug or product bug?**  
A: Don't guess - use `@playwright-failure-analyst` to determine with evidence.
