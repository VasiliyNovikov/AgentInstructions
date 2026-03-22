# AGENTS.md — Base Instructions

This file contains universal agent instructions shared across all repositories. Individual repos reference this file and extend it with project-specific details.

## General Principles

- Make the **smallest possible changes** to accomplish the goal. Surgical, minimal edits only.
- Do not fix unrelated bugs or broken tests. Stay focused on the task at hand.
- Always validate that changes don't break existing behavior by building and running tests.

## Development Workflow

For **non-trivial changes** (anything beyond typos or one-line fixes), follow this mandatory two-phase workflow. **Before implementing any change, always present a complete user-visible plan and obtain approval.** **After implementing any change, always present the actual changes for human review and approval before considering the task complete or running `git commit`, `git push`, or `git merge`.** Trivial changes (typos, single-line fixes) may use an abbreviated planning path that skips the full Phase 1 sub-agent review/refinement loop unless additional review is needed, but they must still show the full plan contents listed in Phase 1 step 5 before implementation and still pass through the Human Review Gate after implementation.

> **Sub-agent model rule:** Always invoke code review sub-agents with the same model as the root agent to ensure review quality matches the planning/implementation quality.

### Phase 1 — Plan + Review

1. **Draft a plan** describing the change: what files are affected, what will be added/modified, and why.
2. **Perform a code review using a sub-agent** to critique the plan for correctness, completeness, adherence to project conventions, and potential risks.
3. **Refine the plan** based on the review feedback.
4. Repeat steps 2–3 until the review passes with no actionable issues, or a maximum of **5 review cycles** (each cycle = one review + one refinement) is reached.
5. **Present the full refined plan to the user for approval before implementation starts.** The plan shown to the user must include:
   - affected files
   - intended additions and modifications
   - rationale for the change
   - validation approach
   - current plan-review status
   - any unresolved actionable issues if the review loop stopped after reaching the 5-cycle cap

   After presenting it:
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

5. **Perform a code review using a sub-agent** to review the implementation diff for bugs, convention violations, and missed edge cases. Include surrounding context (direct callers, interfaces, importing files) and describe the change intent per the Code Review Standards section.
6. If the review surfaces **no actionable issues**, proceed to the Human Review Gate.
7. If the review finds issues, loop back to **Subphase A** (step 1) to address them.

Repeat the outer loop (Subphase A → Subphase B) up to **5 iterations**. If issues remain after 5 iterations, stop and report the unresolved items to the user.

#### Human Review Gate

8. After the agent loop completes (review passes or iteration cap reached), **present the implemented changes to the user for review and approval.** This step is mandatory for every change, including trivial or abbreviated-path edits, and it must happen before the workflow is complete or any `git commit`, `git push`, or `git merge` action is taken.
   - If the user **approves the changes**, the workflow is complete.
   - If the user **suggests changes**, incorporate the feedback and restart Phase 2 from Subphase A step 1.

### Definition of "actionable issues"

An issue is **actionable** if it is a bug, logic error, security vulnerability, missed requirement, convention violation, style inconsistency, resource leak, concurrency defect, breaking API/compatibility change, performance regression, or missing test coverage for new or changed code paths. Optional optimizations and "consider doing X" recommendations are **not** actionable and should not block the workflow, but should be collected and presented to the user at the Human Review Gate for consideration.

### Loop exit rules

- **Exit early from the agent review loop** as soon as the review passes with no actionable findings and all tests pass, then proceed to the Human Review Gate. The task is not complete until the user has reviewed and approved the implemented changes.
- Each iteration must make measurable progress. If an iteration produces no changes, exit the loop and escalate to the user.

## Code Review Standards

When performing a code review (in any phase), evaluate against these dimensions:

- **Correctness** — Verify logic, edge cases, off-by-one errors, null handling, and incorrect assumptions.
- **Security** — Check for injection, auth bypass, data exposure, and secrets in code.
- **Error & Resource Handling** — Confirm no swallowed errors, unhandled exceptions, or leaked resources (connections, file handles, memory).
- **API & Compatibility** — Validate no breaking changes to public APIs, config formats, or data schemas. Verify consistent naming and documented behavior.
- **Concurrency & Performance** — Check for race conditions, deadlocks, unnecessary allocations, and algorithmic inefficiency.
- **Test Adequacy** — Verify new or changed code paths have corresponding test coverage.

**Proportionality:** For changes under 20 lines or affecting a single function, focus the review on correctness, security, error handling, and convention adherence only. Apply the full checklist to larger changes.

**Context requirement:** When invoking the review sub-agent, always include:
1. The diff or changed files.
2. Direct callers, implemented interfaces, and files importing changed symbols.
3. Project-specific conventions from the repository's own `AGENTS.md` (not this base file).
4. A description of the change's intent so the reviewer can validate correctness against it.

## Git Workflow Rules

- **Never commit directly to the default branch.** All changes must go through a separate feature/fix branch and be merged via a pull request.
- **Never run `git commit`, `git push`, or `git merge` until the user has reviewed and approved the implemented changes at the Human Review Gate, and then separately given explicit approval for the specific git operation.** Approval to run a git command does not replace review and approval of the changes themselves.
- Write clear, concise commit messages describing what changed and why.

## Code Quality

- Warnings are errors. Do not suppress warnings without justification.
- Enforce code style at build time when possible.
- Only comment code that needs clarification. Do not add obvious or redundant comments.
- Prefer ecosystem tools (package managers, refactoring tools, linters) over manual changes to reduce mistakes.
- **Avoid duplicated code.** Before adding new code, check for existing functionality that overlaps — both within the new code itself and against the existing codebase. Deduplicate by extracting shared logic into reusable functions, methods, or modules when reasonable.

## Testing

- Run existing linters, builds, and tests before and after making changes.
- Do not add new testing or linting tools unless the task requires it.
- Run tests sequentially if they use shared resources (ports, files, databases).

## Documentation Rules

**After every change, feature, or fix:**
- Review whether `AGENTS.md` and `README.md` need updating. If the change affects documented behavior, conventions, architecture, CI, or public-facing information, update them accordingly.
- Check if dependency manifests need updating.

This is mandatory, not optional.

## Conventions for `AGENTS.md` in Each Repository

Each repo's `AGENTS.md` should document:
1. **Build, Test, and Lint** — exact commands to build, test, and lint the project
2. **Architecture** — project purpose, target frameworks, project structure, key layers, exception hierarchy
3. **CI** — pipeline jobs, matrix, artifacts, publish steps
4. **Code Conventions** — naming, style, patterns enforced in the codebase
5. **Test Conventions** — framework, parallelism constraints, parameterization, custom helpers
