# AGENTS.md — Base Instructions

This file contains universal agent instructions shared across all repositories. Individual repos reference this file and extend it with project-specific details.

## General Principles

- Make the **smallest possible changes** to accomplish the goal. Surgical, minimal edits only.
- Do not fix unrelated bugs or broken tests. Stay focused on the task at hand.
- Always validate that changes don't break existing behavior by building and running tests.

## Git Workflow Rules

- **Never commit directly to the default branch.** All changes must go through a separate feature/fix branch and be merged via a pull request.
- **Always ask for user confirmation before running any `git commit`, `git push`, or `git merge` operation.** Do not execute these commands without explicit approval.
- Write clear, concise commit messages describing what changed and why.

## Code Quality

- Warnings are errors. Do not suppress warnings without justification.
- Enforce code style at build time when possible.
- Only comment code that needs clarification. Do not add obvious or redundant comments.
- Prefer ecosystem tools (package managers, refactoring tools, linters) over manual changes to reduce mistakes.

## Testing

- Run existing linters, builds, and tests before and after making changes.
- Do not add new testing or linting tools unless the task requires it.
- Run tests sequentially if they use shared resources (ports, files, databases).

## Documentation Rules

**After every change, feature, or fix:**
- Review and update `AGENTS.md` and `README.md` if the change affects documented behavior, conventions, architecture, CI, or public-facing information.
- Check if dependency manifests need updating.

This is mandatory, not optional.

## Conventions for `AGENTS.md` in Each Repository

Each repo's `AGENTS.md` should document:
1. **Build, Test, and Lint** — exact commands to build, test, and lint the project
2. **Architecture** — project purpose, target frameworks, project structure, key layers, exception hierarchy
3. **CI** — pipeline jobs, matrix, artifacts, publish steps
4. **Code Conventions** — naming, style, patterns enforced in the codebase
5. **Test Conventions** — framework, parallelism constraints, parameterization, custom helpers
