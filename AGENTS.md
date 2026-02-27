# AGENTS.md — Base Instructions

This file contains universal agent instructions shared across all repositories. Individual repos reference this file and extend it with project-specific details.

## General Principles

- Make the **smallest possible changes** to accomplish the goal. Surgical, minimal edits only.
- Do not fix unrelated bugs or broken tests. Stay focused on the task at hand.
- Always validate that changes don't break existing behavior by building and running tests.

## Development Workflow

For **non-trivial changes** (anything beyond typos or one-line fixes), follow this mandatory two-phase workflow. Trivial changes (typos, single-line fixes) may skip directly to implementation.

### Phase 1 — Plan + Review

1. **Draft a plan** describing the change: what files are affected, what will be added/modified, and why.
2. **Launch a `code-review` subagent** to critique the plan for correctness, completeness, adherence to project conventions, and potential risks.
3. **Refine the plan** based on the review feedback.
4. Repeat steps 2–3 until the review passes with no actionable issues, or a maximum of **5 review cycles** (each cycle = one review + one refinement) is reached.
5. **Present the plan to the user for approval.**
   - If the user **approves**, proceed to Phase 2.
   - If the user **suggests changes**, incorporate the feedback and restart Phase 1 from step 1.

### Phase 2 — Implement + Test + Review (up to 5 outer iterations)

Each outer iteration consists of two subphases:

#### Subphase A — Implement + Test (inner loop)

1. **Implement** the changes according to the approved plan (or fix issues from the previous review).
2. **Build and run tests**.
3. If tests fail, **fix** the failures and repeat from step 2.
4. Continue until all tests pass. This inner loop has no fixed iteration cap but must make progress on each iteration.

#### Subphase B — Review

5. **Launch a `code-review` subagent** to review the implementation diff for bugs, convention violations, and missed edge cases.
6. If the review surfaces **no actionable issues**, the workflow is complete — exit.
7. If the review finds issues, loop back to **Subphase A** (step 1) to address them.

Repeat the outer loop (Subphase A → Subphase B) up to **5 iterations**. If issues remain after 5 iterations, stop and report the unresolved items to the user.

#### Human Review Gate

8. After the agent loop completes (review passes or iteration cap reached), **present the changes to the user for approval.**
   - If the user **approves**, the workflow is complete.
   - If the user **suggests changes**, incorporate the feedback and restart Phase 2 from Subphase A step 1.

### Definition of "actionable issues"

An issue is **actionable** if it is a bug, logic error, security vulnerability, missed requirement, convention violation, or style inconsistency. Optional optimizations and "consider doing X" recommendations are **not** actionable and should not block the workflow, but should be collected and presented to the user at the Human Review Gate for consideration.

### Loop exit rules

- **Exit early** as soon as the review passes with no actionable findings and all tests pass. Do not iterate unnecessarily.
- Each iteration must make measurable progress. If an iteration produces no changes, exit the loop and escalate to the user.

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
