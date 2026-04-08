# TypeScript Development Suite

A comprehensive TypeScript development workflow plugin for Claude Code, providing specialized agents for writing, testing, and reviewing TypeScript code.

## What's Included

### Agents

#### ts-engineer
Expert TypeScript developer specializing in idiomatic patterns and best practices. Implements new features, refactors code, and optimizes existing implementations with a focus on type safety, maintainability, and testability.

**Best for:** Writing new TypeScript code, refactoring, implementing features, type system optimization

#### ts-tester
Specialized in creating and maintaining comprehensive TypeScript tests (unit, integration, and E2E). Advocates for testability and helps improve test coverage across your codebase using modern testing frameworks like Jest, Vitest, or Mocha.

**Best for:** Writing tests, improving coverage, debugging test failures, async test patterns

#### ts-code-reviewer
Performs thorough code reviews focusing on correctness, type safety, maintainability, performance, and TypeScript best practices. Provides actionable feedback and suggestions for improvement with special attention to async patterns and security.

**Best for:** Pre-commit reviews, PR reviews, code quality audits, type safety checks

#### ts-comment-reviewer
Focuses specifically on comment quality, ensuring comments explain "why not what" and are used sparingly and effectively. Includes JSDoc standards for public APIs.

**Best for:** Documentation review, comment quality checks, JSDoc compliance

### Commands

#### /ts-code
Orchestrates the complete TypeScript development workflow:
1. Implements code changes with ts-engineer
2. Creates/updates tests with ts-tester
3. Reviews comments with ts-comment-reviewer
4. Performs final code review with ts-code-reviewer

**Usage:** `/ts-code implement user authentication with JWT tokens and proper type safety`

#### /ts-test
Focused workflow for test development:
1. Updates tests with ts-tester
2. Reviews comments with ts-comment-reviewer
3. Performs code review with ts-code-reviewer

**Usage:** `/ts-test add integration tests for the API endpoints with proper mocking`

## Installation

```bash
# Add the spoton-ai-dev-enablement marketplace
/plugin marketplace add https://github.com/SpotOnInc/spoton-ai-dev-enablement

# Install this plugin
/plugin install ts-dev@spoton-ai-dev-enablement
```

## Usage Examples

### Writing New Code
```bash
# Ask Claude to use the agent
Use the ts-engineer agent to implement a rate limiter using TypeScript generics and proper async patterns
```

### Creating Tests
```bash
# Ask Claude to use the agent
Use the ts-tester agent to create comprehensive unit tests for the authentication service with proper mocking
```

### Code Review
```bash
# Ask Claude to use the agent
Use the ts-code-reviewer agent to review the changes in src/auth/jwt.ts for type safety and security
```

### Complete Workflow
```bash
# Use the slash command for the full workflow
/ts-code add user registration with email verification and proper TypeScript types
```

## Best Practices

1. **Use /ts-code for feature work** - It provides the complete workflow from implementation to testing to review
2. **Ask Claude to use ts-engineer directly** - When you just need implementation without the full workflow
3. **Ask Claude to use ts-tester for test-first development** - Write tests before implementation with proper TypeScript typing
4. **Ask Claude to run ts-code-reviewer before PRs** - Catch type safety issues and async bugs early in your workflow
5. **Leverage TypeScript's strict mode** - All agents work best with strict TypeScript configuration

## Framework Support

This plugin works with popular TypeScript frameworks and tools:
- **Testing:** Jest, Vitest, Mocha, Chai, Testing Library
- **Frontend:** React, Vue, Angular, Svelte
- **Backend:** Node.js, Express, Fastify, NestJS
- **Build Tools:** Webpack, Vite, esbuild, Rollup
- **Linting:** ESLint, Prettier, TypeScript compiler

## License

MIT