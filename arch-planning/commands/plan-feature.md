---
description: Full planning pipeline from a spec file to a TypeScript-ready task breakdown. Runs requirements-analyst → software-architect → technical-design-architect → task-breakdown-lead, with back-communication allowed at each stage.
---

Read the spec file provided and run the full planning pipeline:

**Spec file:** `$ARGUMENTS`

If no file path is provided, or the file does not exist, stop and ask the user for a valid spec file path.

---

## Setup

Before starting:

1. Read the spec file at the path provided in `$ARGUMENTS`
2. Derive a short kebab-case feature name from the spec (e.g., `user-authentication`, `payment-flow`, `dashboard-export`)
3. Create an output folder: `./{feature-name}-plan/`
4. All agent outputs will be saved as numbered files in that folder

---

## Pipeline

### Stage 1 — Requirements Analysis

**Use requirements-analyst** to analyze the spec file and produce a requirements document.

- Pass the full contents of the spec file as context
- Agent may ask clarifying questions — answer them or relay them to the user as needed
- When complete, save output to: `./{feature-name}-plan/01-requirements.md`

**Back-communication:** If at any later stage an agent identifies a gap or ambiguity in the requirements, return to requirements-analyst with the specific question, update `01-requirements.md` with the revised output, then continue forward from that stage.

---

### Stage 2 — Architecture

**Use software-architect** to evaluate architectural approaches based on `01-requirements.md`.

- Pass `01-requirements.md` as primary input
- Agent should focus on TypeScript/Node.js stack decisions, frontend framework choices, data layer, and integration patterns consistent with the existing codebase (if any)
- If the architect identifies ambiguities in the requirements, route the question back to requirements-analyst (Stage 1 back-communication), then resume
- When complete, save output to: `./{feature-name}-plan/02-architecture.md`

---

### Stage 3 — Technical Design

**Use technical-design-architect** to produce a detailed technical specification based on `01-requirements.md` and `02-architecture.md`.

- Pass both prior documents as input
- The design **must be TypeScript-specific**: include concrete interface definitions, type aliases, key function signatures, module/file structure, and testing approach (Jest or Vitest)
- If the designer identifies ambiguities in the architecture, route the question back to software-architect (Stage 2 back-communication), then resume
- When complete, save output to: `./{feature-name}-plan/03-technical-design.md`

The technical design must include:

```
- TypeScript interfaces and types for all major data structures
- Module and file structure (file paths, what each file exports)
- API contracts (if applicable) with request/response types
- State management approach (if applicable)
- External dependencies and why each is chosen
- Testing strategy: what to unit test, what to integration test
```

---

### Stage 4 — Task Breakdown

**Use task-breakdown-lead** to convert the technical design into an ordered, dependency-aware task list.

- Pass `01-requirements.md`, `02-architecture.md`, and `03-technical-design.md` as input
- If the lead identifies ambiguities in the technical design, route the question back to technical-design-architect (Stage 3 back-communication), then resume
- Tasks must be written so that each one can be passed directly to `/ts-code` or `/ts-test` in the ts-dev plugin as a self-contained unit of work
- When complete, save output to: `./{feature-name}-plan/04-tasks.md`

Each task in `04-tasks.md` must follow this format so ts-dev agents understand it:

```md
- [ ] N. [Verb] [what] in [file/module]
  - TypeScript context: interfaces involved, imports needed
  - Acceptance: specific behavior that makes this task complete
  - Test requirement: what ts-tester should verify
  - _Requires: [prior task numbers]_
  - _Blocks: [dependent task numbers]_
```

Examples of well-formed tasks for ts-dev:
- `Implement UserRepository interface and InMemoryUserRepository in src/repositories/user.ts`
- `Add POST /auth/login endpoint in src/routes/auth.ts using AuthService and returning AuthToken type`
- `Write unit tests for AuthService.login() covering success, invalid credentials, and locked account cases`

---

### Stage 5 — Output Summary

Create `./{feature-name}-plan/README.md` with:

```md
# {Feature Name} — Plan

## Spec
Source: {path to original spec file}

## How to use this plan with ts-dev

Run tasks in the order listed in `04-tasks.md`. For each task:

- Implementation tasks → `/ts-code {paste task description and details}`
- Test-only tasks → `/ts-test {paste task description and details}`

Tasks marked `⟳ parallel` can be assigned to different developers simultaneously.

## Pipeline Output
- `01-requirements.md` — Functional and non-functional requirements
- `02-architecture.md` — Architecture decisions and rationale
- `03-technical-design.md` — TypeScript interfaces, module structure, API contracts
- `04-tasks.md` — Ordered, dependency-annotated implementation tasks

## Assumptions and Open Questions
{List any unresolved items that the user should address before or during implementation}
```

---

## Back-Communication Rules

When an agent at Stage N has a question about output from Stage N-1:

1. State the question clearly (don't proceed with assumptions)
2. Re-invoke the earlier agent with the specific question
3. Update the earlier stage's output file with any changes
4. Resume from Stage N with the updated input

An agent should ask a back-communication question when:
- A requirement is ambiguous enough to produce two meaningfully different designs
- An architectural decision is missing that blocks the technical design
- A type or interface is undefined in the design but required for the task breakdown

An agent should **not** ask a back-communication question when:
- A reasonable default exists and can be documented as an assumption
- The question is about implementation style rather than correctness
- The spec file itself answers the question

---

## Important Notes

- **TypeScript focus**: The entire pipeline should produce output optimized for a TypeScript codebase. If the spec mentions no specific tech stack, default to TypeScript with Node.js for backend and React for frontend.
- **ts-dev compatibility**: Every task in `04-tasks.md` must be actionable by ts-engineer, ts-tester, and ts-code-reviewer without additional context beyond what's in the task entry and the technical design.
- **One folder per run**: All output goes into `./{feature-name}-plan/`. If the folder already exists, ask the user whether to overwrite or create a new versioned folder (e.g., `{feature-name}-plan-v2/`).
- **Save incrementally**: Save each stage's output file as soon as that stage completes. Don't wait until the end.
