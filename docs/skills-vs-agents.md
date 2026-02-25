# Skills vs agents
The line between skills and agents can be kind of blurry. 

### Skills (in claude.md or referenced files) are best for:

Persistent knowledge that should always be available — coding conventions, project context, architectural patterns
Lightweight guidance — "when writing API endpoints, always include input validation and rate limiting"
Things Claude should internalize, not execute as a separate task
They're essentially instructions that shape how Claude thinks and works within the main conversation

### Subagents (.claude/agents/) are best for:

Discrete, repeatable workflows that have a clear input → output — like "review this PR," "write a migration," "generate API docs from code"
Tasks you'd delegate to a junior dev with specific instructions — they run as isolated conversations with their own system prompt
Complex multi-step procedures where you want focused execution without polluting your main context
Parallelizable work — you can kick off multiple subagents simultaneously.


# Claude Code Subagents Reference

A catalog of reusable subagents (`.claude/agents/`) for Claude Code. Each entry includes guidance on how broadly it applies, how quickly it can be adopted, and whether it works best as tech-agnostic or tech-specific.

---

## PR Reviewer

Analyzes a diff or pull request for bugs, style violations, security concerns, missing tests, and suggests improvements.

- **Applicability:** General — every team does code review regardless of stack.
- **Adoptability:** Immediate — works out of the box with minimal configuration. Point it at a diff and it returns useful feedback on day one.
- **Tech specific or agnostic:** Start tech-agnostic. It can catch logic errors, naming issues, and missing edge cases in any language. Layer on tech-specific rules over time (e.g., React hook dependency checks, SQL injection patterns).

---

## Test Writer

Takes a file or function, generates a test suite, runs it, and iterates until tests pass.

- **Applicability:** General — testing is universal across all codebases.
- **Adoptability:** Immediate — a basic version that writes unit tests for pure functions works instantly. Improves significantly once you configure it with your test framework and conventions.
- **Tech specific or agnostic:** Best as tech-specific. The test runner, assertion library, mocking approach, and file placement conventions vary across stacks. A generic version still adds value, but a configured one is dramatically better.

---

## Commit Message / PR Description Writer

Reads staged changes or a diff and produces a well-structured commit message or PR description following your conventions.

- **Applicability:** General — every team writes commits and PRs.
- **Adoptability:** Immediate — almost zero configuration needed. Add your commit convention (conventional commits, etc.) and it's ready.
- **Tech specific or agnostic:** Tech-agnostic. Commit conventions are about communication, not technology. Optional enhancement: link to ticketing systems (Jira, Linear) for richer context.

---

## Bug Investigator

Given an error message, stack trace, or bug report, traces through the codebase to identify the root cause and suggest a fix.

- **Applicability:** General — every team debugs issues.
- **Adoptability:** Medium — useful immediately for straightforward errors, but becomes much more powerful once it understands your project's architecture, logging patterns, and common failure modes.
- **Tech specific or agnostic:** Start tech-agnostic, then specialize. A generic version can read stack traces and grep through code. A specialized version can query your observability tools (Datadog, Sentry), understand your error handling patterns, and check known issue databases.

---

## Documentation Writer

Reads code and generates or updates documentation — READMEs, API docs, inline comments, architecture docs.

- **Applicability:** General — documentation is universally needed and universally neglected.
- **Adoptability:** Immediate — even a naive version that generates a first draft from code saves significant time. No configuration required to start.
- **Tech specific or agnostic:** Start tech-agnostic. Becomes more valuable as tech-specific when you need particular output formats (OpenAPI specs, JSDoc with custom tags, Storybook stories, man pages).

---

## Security Auditor

Scans code for common vulnerabilities: injection, auth issues, exposed secrets, insecure dependencies, misconfigurations.

- **Applicability:** General — security applies to every codebase.
- **Adoptability:** Immediate — basic checks (hardcoded secrets, SQL injection, missing input validation) are universally applicable. More advanced checks require tuning.
- **Tech specific or agnostic:** Both. Start tech-agnostic for universal patterns (secrets, auth, input validation). Add tech-specific rules for high-value checks: Rails mass assignment, Next.js server action leaks, Django ORM injection, CORS misconfigurations per framework. Compliance-specific versions (SOC2, HIPAA, PCI-DSS) are inherently specialized.

---

## Issue / Ticket Drafter

Takes a rough description, conversation, or bug report and produces a well-structured ticket with acceptance criteria, scope, and technical context.

- **Applicability:** General — every team writes tickets, and quality varies wildly.
- **Adoptability:** Immediate — useful from day one for anyone who writes tickets. Add your template and it's standardized across the org.
- **Tech specific or agnostic:** Tech-agnostic. Ticket writing is about communication and scoping, not technology. Optional: integrate with project management tools (Jira, Linear, Asana) for richer context.

---

## Changelog Generator

Reads commits or PRs since the last release and produces a formatted changelog.

- **Applicability:** General — any team that ships releases benefits from changelogs.
- **Adoptability:** Immediate — reads git history and categorizes changes. Works as soon as your team follows any semblance of commit conventions.
- **Tech specific or agnostic:** Tech-agnostic. The output format might vary (CHANGELOG.md, GitHub release notes, Slack message), but the logic is stack-independent. Optional: categorize by audience (internal vs. customer-facing) or integrate with release tooling.

---

## Onboarding Guide

Answers questions about the codebase by reading code, docs, and architecture to help new developers get oriented.

- **Applicability:** General — every team onboards people and every codebase has tribal knowledge.
- **Adoptability:** Immediate — even without configuration, it can answer "where does X happen" or "how does auth work" by reading the code. Gets better with architecture docs and context in `claude.md`.
- **Tech specific or agnostic:** Tech-agnostic. The value is in navigating *your* codebase, not in knowing a particular framework. Works for any stack.

---

## Refactorer

Takes a file or module and refactors it according to specified patterns — extract function, simplify conditionals, improve naming, reduce duplication.

- **Applicability:** General — every codebase accumulates tech debt.
- **Adoptability:** Medium — useful immediately for simple refactors (rename, extract, simplify). Higher value once configured with your org's target patterns and architectural direction.
- **Tech specific or agnostic:** Best as tech-specific. Refactoring patterns are tied to language idioms and framework conventions (e.g., "convert class components to hooks," "replace callbacks with async/await," "migrate from Redux to Zustand"). A generic version still helps, but a stack-aware one is significantly more effective.

---

## Migration Generator

Generates database migrations, API version bumps, or config migrations from a description of the desired change.

- **Applicability:** Niche — tightly coupled to your ORM, migration framework, and database.
- **Adoptability:** Long-term — requires understanding of your schema conventions, naming patterns, rollback strategy, and data migration approach. Significant upfront investment to configure well.
- **Tech specific or agnostic:** Tech-specific. This is inherently tied to your stack (Prisma, Alembic, ActiveRecord, Flyway, etc.). The value comes entirely from encoding *your* migration patterns.

---

## Code Scaffolder

Generates boilerplate for new features: API endpoint + handler + test + types + migration stub, all wired together.

- **Applicability:** Niche — the entire point is encoding your project's specific conventions and file structure.
- **Adoptability:** Long-term — requires significant upfront effort to define templates and patterns. But once configured, it's one of the highest-leverage subagents for velocity. Every new feature starts consistent and correct.
- **Tech specific or agnostic:** Tech-specific. This is deeply tied to your framework, file structure, and conventions. A scaffolder for a Next.js app looks nothing like one for a Django app. Not portable, but extremely valuable within a project.

---

## Dependency Updater

Reviews outdated dependencies, checks changelogs for breaking changes, and proposes updates with necessary code changes.

- **Applicability:** General — every project has dependencies that go stale.
- **Adoptability:** Medium — basic version (list outdated, check changelogs) works immediately. Full version (automated updates with code changes and test verification) requires understanding your CI pipeline, monorepo structure, and risk tolerance.
- **Tech specific or agnostic:** Both. Listing outdated packages is tech-agnostic. Actually performing updates, handling breaking changes, and verifying compatibility is tech-specific (npm vs. pip vs. cargo, lockfile handling, peer dependency resolution).

---

## Quick Reference

| Subagent | Applicability | Adoptability | Best As |
|---|---|---|---|
| PR Reviewer | General | Immediate | Start agnostic, layer specifics |
| Test Writer | General | Immediate | Tech-specific |
| Commit/PR Description Writer | General | Immediate | Tech-agnostic |
| Bug Investigator | General | Medium | Start agnostic, specialize |
| Documentation Writer | General | Immediate | Start agnostic, specialize |
| Security Auditor | General | Immediate | Both |
| Issue/Ticket Drafter | General | Immediate | Tech-agnostic |
| Changelog Generator | General | Immediate | Tech-agnostic |
| Onboarding Guide | General | Immediate | Tech-agnostic |
| Refactorer | General | Medium | Tech-specific |
| Migration Generator | Niche | Long-term | Tech-specific |
| Code Scaffolder | Niche | Long-term | Tech-specific |
| Dependency Updater | General | Medium | Both |