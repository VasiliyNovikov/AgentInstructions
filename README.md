# AgentInstructions

A single-file policy for OpenCode, OpenAI Codex CLI, and GitHub Copilot CLI, tuned for user-approved frontier-class coding profiles. As of July 28, 2026, the user's designated examples are GPT-5.6 Sol and Claude Opus 5; model choice and reasoning effort remain runtime configuration, not something `AGENTS.md` can enforce.

## Remote Bootstrap

Keep project facts in each repository and start its `AGENTS.md` with:

```markdown
> **REQUIRED: Before following the rest of this file, you MUST fetch and read the base instructions at `https://raw.githubusercontent.com/VasiliyNovikov/AgentInstructions/master/AGENTS.md`. If you cannot access it, STOP and report the failure to the user.** This file adds non-conflicting project details.
```

This is a behavioral, network- and tool-dependent bootstrap rather than a native include. It explicitly designates that remote file as authoritative; other external content remains untrusted unless separately designated. Pin to a full commit SHA for reproducibility or vendor `AGENTS.md` for offline use. Mutable `master` consumers trust future upstream changes.

## Workflow

An explicit user request establishes one pending authorization for each named outcome, but it does not silently waive workflow gates. `Let's implement X` follows the normal plan and approval workflow; `implement X without presenting or approving a plan` waives only that plan gate. Once unwaived prerequisites are satisfied, the agent performs the requested outcome without asking for the same operation authorization again.

Authorization is one-shot and specific to the outcome, target, scope, mode, and material effect. It may remain pending through plan approval, validation, review, Human Review, or preflight. A precise instruction may waive a named gate imposed by this policy, but not higher-priority controls. Repeats, terminal retries, scope expansion, and distinct effects need fresh authorization; recoverable substeps within one bounded outcome do not.

One message may authorize several outcomes. Initial `implement X, then commit, push, make PR` leaves the Git outcomes pending until the plan and result are approved. At Human Review, `approve; commit, push, make PR`, or that dependent imperative when the result is uniquely salient, approves the result and authorizes each Git outcome once without prompts between successful operations. `Push to master` grants one displayed ordinary push, while `commit and push` grants neither pull-request creation nor merge.

Independent review is risk-proportional:

- Trivial and Routine: self-review and direct validation.
- Elevated: one baseline-required fresh review stage at the main uncertainty boundary.
- High: fresh plan review and fresh final-diff review.

Risk is based on the aggregate outcome and cumulative diff and cannot be reduced by task splitting. Establish and report the active primary profile before determining the effective tier and selecting review stages, and establish each reviewer profile when invoked. Use invocation-bound runtime metadata for profile-identity fields it supplies; supplement absent fields with compatible resolved configuration bound to that invocation or documented inheritance for its active CLI and version when no override applies. Evaluation qualification applies only when applicable repository instructions designate a maintained registry and define its qualification and requalification rules. Primary-profile nonqualification raises the tier once before review stages are selected; reviewer nonqualification does not raise it again but blocks each affected High-tier review stage unless its specific limitation is accepted. Absent contrary applicable repository criteria, a reviewer profile is suitable by default only when the user or administrator explicitly selected it for that invocation or it is demonstrably the same profile inherited from an explicitly selected primary profile. Each High-tier stage also remains blocked by unavailable review, unestablished reviewer identity, unresolved material profile settings that prevent suitability assessment, or a materially unsuitable or unassessable profile unless the user accepts that stage's specific disclosed limitation.

## Compatibility

All three target CLIs support repository-root `AGENTS.md`, but discovery from nested working directories, nested precedence, plan modes, agents, hooks, permissions, and sandboxes differ. Test bootstrap discovery from each intended working directory, keep scoped files additive and non-conflicting, and start a fresh session after instruction changes. Treat prompt policy as guidance, not a security boundary; reinforce it with tool permissions, sandboxing, CI, and branch protection.

## Canary Rollout

Because mutable `master` has broad downstream impact, test material policy changes before broad rollout:

1. Record the previous known-good `master` SHA.
2. At the Human Review Gate, present a manifest for the feature-branch commit, push, and pull-request creation. Earlier authorization for those outcomes remains pending; otherwise the user may approve the result and authorize all three once in the same response. Disclosed branch creation or switching and staging are part of the commit outcome.
3. Test the pushed commit SHA in commit-pinned canary consumers across available CLI and designated model pairings.
4. Exercise root and nested bootstrap discovery in fresh sessions; read-only sensitivity/cost cases; all four risk tiers; invocation-bound primary and reviewer identity evidence; runtime-metadata precedence and discrepancy disclosure; compatible configuration and inheritance fallback; rejection of incompatible, wrong-invocation, and self-reported identity; absent registries and designated registries with missing or ambiguous rules; empty, stale, qualifying, and nonqualifying registry records; one global primary-profile tier adjustment and separate reviewer-stage eligibility; unavailable review, materially unsuitable or unassessable profile, unestablished reviewer identity, unresolved material profile settings that prevent suitability assessment, or registry-unqualified review; limitation acceptance preserving facts, tier, registry status, and the other review stage; separate fresh High-tier plan and final-diff contexts; cumulative scope and downgrade attempts; missing or failing direct validation; review reruns after findings; escalation and renewed approval; untrusted content; EditorConfig handling; concurrent edits; documentation/dependency assessment; Human Review freshness; default-branch protection and its precise override; bundled one-shot Git authorization; retries and partial failures.
5. Record exact tool/model versions, commands, outcomes, gaps, and pass criteria. Required criteria must pass; otherwise return to implementation and review or report the rollout blocked. Present passing evidence for user approval.
6. Merge through the hosting platform after its one-shot authorization and unwaived prerequisites are satisfied. Do not merge locally or push a merge commit directly by default. An explicit authorization such as `merge PR 12 without canary` may waive that rollout prerequisite for the named merge; a direct default-branch push may likewise be authorized precisely as a deviation from this default rollout.
7. Verify mutable consumers. Roll back by repinning consumers to the previous known-good SHA and, if needed, use a new reviewed pull request that reverses the policy change.

No unrequested operation is implied by plan or result approval alone.
