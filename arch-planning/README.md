# Architecture Planning

A comprehensive suite of agents for requirements analysis, system architecture design, technical specifications, and task breakdown. Perfect for planning and designing software projects before implementation.

## What's Included

### Agents

#### requirements-analyst
Transforms high-level ideas and requests into comprehensive, actionable requirements. Excels at gathering details, clarifying ambiguities, and documenting functional and non-functional requirements.

**Best for:** Early project phases, feature planning, requirement gathering

#### software-architect
Provides expert guidance on software architecture decisions, system design, and technology stack recommendations. Challenges assumptions and provides pragmatic alternatives based on real-world constraints.

**Best for:** Architecture decisions, system design, technology selection

#### technical-design-architect
Creates comprehensive technical designs from completed requirements documents. Produces detailed specifications including architecture overview, component design, data models, API specifications, and implementation guidelines.

**Best for:** Detailed technical specifications, design documentation

#### task-breakdown-lead
Breaks down requirements and technical designs into implementable development tasks with proper sequencing, dependencies, and risk mitigation strategies.

**Best for:** Sprint planning, project breakdown, task estimation

### Commands

#### /plan-feature
Full planning pipeline from a spec file to a TypeScript-ready task breakdown. Runs the complete agent chain — requirements-analyst → software-architect → technical-design-architect → task-breakdown-lead — with back-communication allowed at each stage. Output is saved as numbered files in a new folder, formatted so the ts-dev plugin can implement the feature directly.

**Usage:** `/plan-feature path/to/spec.md`

**Output:** `./{feature-name}-plan/` folder containing:
- `01-requirements.md` — requirements document
- `02-architecture.md` — architecture decisions
- `03-technical-design.md` — TypeScript interfaces, module structure, API contracts
- `04-tasks.md` — ordered task list ready for `/ts-code` and `/ts-test`
- `README.md` — summary and usage instructions

#### /review-repo
Analyzes repository architecture, patterns, and structure to provide insights into the codebase organization and design decisions.

**Usage:** `/review-repo analyze the microservices architecture`

## Installation

```bash
# Add the spoton-ai-dev-enablement marketplace
/plugin marketplace add https://github.com/SpotOnInc/spoton-ai-dev-enablement

# Install this plugin
/plugin install arch-planning@spoton-ai-dev-enablement
```

## Usage Examples

### Requirements Gathering
```bash
# Ask Claude to use the agent
Use the requirements-analyst agent to help me define requirements for a multi-tenant SaaS application
```

### Architecture Design
```bash
# Ask Claude to use the agent
Use the software-architect agent to evaluate architecture options for a real-time notification system
```

### Technical Specifications
```bash
# Ask Claude to use the agent
Use the technical-design-architect agent to create a technical design for the user authentication service based on the requirements doc
```

### Task Breakdown
```bash
# Ask Claude to use the agent
Use the task-breakdown-lead agent to break down the authentication service design into development tasks
```

### Repository Analysis
```bash
# Use the slash command
/review-repo
```

## Workflow Example

Follow this workflow for comprehensive project planning:

1. **Requirements** - Ask Claude to use requirements-analyst to gather and document requirements
2. **Architecture** - Ask Claude to use software-architect to evaluate and decide on architecture approaches
3. **Design** - Ask Claude to use technical-design-architect to create detailed technical specifications
4. **Planning** - Ask Claude to use task-breakdown-lead to break down the design into implementable tasks

```bash
# Option A: Run the full pipeline automatically from a spec file
/plan-feature path/to/spec.md

# Option B: Run agents individually
# Step 1: Gather requirements
Use the requirements-analyst agent - I want to build a real-time collaboration feature

# Step 2: Evaluate architecture
Use the software-architect agent to determine the best approaches for real-time collaboration with 1000+ concurrent users

# Step 3: Create technical design
Use the technical-design-architect agent to create a detailed technical design based on the requirements and architecture decisions

# Step 4: Break down into tasks
Use the task-breakdown-lead agent to break down the technical design into sprint-ready tasks
```

## Best Practices

1. **Start with requirements** - Always begin by asking Claude to use requirements-analyst to ensure you're building the right thing
2. **Challenge your assumptions** - Ask Claude to use software-architect to evaluate alternatives and identify potential issues early
3. **Document thoroughly** - Ask Claude to use technical-design-architect to create detailed specifications that guide implementation
4. **Plan incrementally** - Ask Claude to use task-breakdown-lead to create manageable, sequenced tasks with clear dependencies

## License

MIT
