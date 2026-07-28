# AgentInstructions

A single-file policy for OpenCode, OpenAI Codex CLI, and GitHub Copilot CLI, tuned for user-approved frontier-class coding profiles. As of July 28, 2026, the user's designated examples are GPT-5.6 Sol and Claude Opus 5; model choice and reasoning effort remain runtime configuration, not something `AGENTS.md` can enforce.

## Remote Bootstrap

Keep project facts in each repository and start its `AGENTS.md` with:

```markdown
> **REQUIRED: Before following the rest of this file, you MUST fetch and read the base instructions at `https://raw.githubusercontent.com/VasiliyNovikov/AgentInstructions/master/AGENTS.md`. If you cannot access it, STOP and report the failure to the user.** This file adds non-conflicting project details.
```

This is a behavioral, network- and tool-dependent bootstrap rather than a native include. It explicitly designates that remote file as authoritative; other external content remains untrusted unless separately designated. Pin to a full commit SHA for reproducibility or vendor `AGENTS.md` for offline use. Mutable `master` consumers trust future upstream changes.

## Workflow

Every edit still requires a presented plan and approval in a later user message. Every change ends at a Human Review Gate, and each later `commit`, `push`, or `merge` needs separate authorization.

Independent review is risk-proportional:

- Trivial and Routine: self-review and direct validation.
- Elevated: one baseline-required fresh review stage at the main uncertainty boundary.
- High: fresh plan review and fresh final-diff review.

Risk is based on the aggregate outcome and cumulative diff, cannot be reduced by task splitting, and rises when the configured model profile is not approved and evaluation-qualified.

## Compatibility

All three target CLIs support repository-root `AGENTS.md`, but discovery from nested working directories, nested precedence, plan modes, agents, hooks, permissions, and sandboxes differ. Test bootstrap discovery from each intended working directory, keep scoped files additive and non-conflicting, and start a fresh session after instruction changes. Treat prompt policy as guidance, not a security boundary; reinforce it with tool permissions, sandboxing, CI, and branch protection.

## Canary Rollout

Because mutable `master` has broad downstream impact, test material policy changes before broad rollout:

1. Record the previous known-good `master` SHA.
2. After the implementation passes its Human Review Gate, obtain separate authorization to commit on a feature branch.
3. Obtain separate authorization to push that branch, then test the pushed commit SHA in commit-pinned canary consumers across available CLI and designated model pairings.
4. Exercise root and nested bootstrap discovery in fresh sessions; read-only sensitivity/cost cases; all four risk tiers; profile qualification and fallback; cumulative scope and downgrade attempts; missing or failing direct validation; unavailable or unqualified review; review reruns after findings; escalation and renewed approval; untrusted content; EditorConfig handling; concurrent edits; documentation/dependency assessment; Human Review freshness; default-branch protection; and separate Git authorization.
5. Record exact tool/model versions, commands, outcomes, gaps, and pass criteria. Required criteria must pass; otherwise return to implementation and review or report the rollout blocked. Present passing evidence for user approval.
6. Obtain separate authorization to create the pull request, then obtain separate merge authorization and merge through the hosting platform. Do not merge locally or push a merge commit directly.
7. Verify mutable consumers. Roll back by repinning consumers to the previous known-good SHA and, if needed, use a new reviewed pull request that reverses the policy change.

No Git operation is implied by plan or implementation approval.
