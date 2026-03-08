# CLAUDE.md — sense-collective

This file provides guidance for AI assistants (Claude and others) working in this repository.

---

## Repository Overview

**Project:** sense-collective
**Status:** Freshly initialized — project structure is being established.

This file will grow as the codebase evolves. Update it whenever you add new tooling, conventions, or architectural decisions.

---

## Development Workflow

### Branching Strategy

- All AI-assisted work happens on a dedicated `claude/<task-id>` branch.
- Never push directly to `main` without explicit permission.
- Use `git push -u origin <branch-name>` when pushing a new branch.

### Commit Messages

Use clear, imperative commit messages:
```
Add user authentication module
Fix off-by-one error in pagination logic
Refactor data fetching to use async/await
```

Avoid vague messages like "update" or "fix stuff".

### Pull Requests

- Keep PRs focused on a single concern.
- Include a summary of *what* changed and *why*.
- Reference the relevant issue number when applicable.

---

## Code Conventions

> Update this section as the project stack is decided and code is written.

### General

- Prefer clarity over cleverness.
- Avoid over-engineering — implement only what the current task requires.
- Do not add comments for self-evident code; only comment non-obvious logic.
- Do not add error handling for scenarios that cannot occur.

### Security

- Never hardcode secrets or credentials. Use environment variables.
- Validate all data at system boundaries (user input, external APIs).
- Avoid common vulnerabilities: SQL injection, XSS, command injection, SSRF.

---

## Project Structure

> To be filled in once the project structure is established.

```
sense-collective/
├── CLAUDE.md          # This file
├── README.md          # (to be created) Human-facing project overview
└── ...                # Source code, tests, config (to be added)
```

---

## Environment & Configuration

- Copy `.env.example` to `.env` for local development (add this file when secrets are needed).
- Never commit `.env` files.

---

## Testing

> To be filled in once a test framework is chosen.

- Run all tests before committing.
- Aim for meaningful tests on business logic, not trivial coverage.

---

## Dependency Management

> To be filled in once a language/runtime is chosen (npm, pip, cargo, etc.).

---

## AI Assistant Guidelines

1. **Read before editing.** Always read the relevant file(s) before making changes.
2. **Stay in scope.** Only make changes directly requested or clearly necessary.
3. **No file bloat.** Prefer editing existing files over creating new ones.
4. **No backwards-compat hacks.** Remove unused code rather than leaving stubs.
5. **Update this file** when you introduce new tooling, structure, or conventions.
6. **Ask before destructive actions.** Deleting files, force-pushing, dropping data — confirm first.
7. **Commit and push.** When a task is complete, commit with a clear message and push to the designated branch.
