# AgentInstructions

A shared `AGENTS.md` for coding agents that aims to be safe, practical, and readable by humans. It is designed for frontier coding models such as [GPT-5.6 Sol](https://developers.openai.com/api/docs/models/gpt-5.6-sol) and [Claude Opus 5](https://platform.claude.com/docs/en/models/opus-5/overview) when used through OpenCode, GitHub Copilot, or OpenAI Codex.

The policy does not select a model, reasoning level, sandbox, or permission mode. Those remain harness and runtime settings.

## Remote Bootstrap

For interactive environments with agent-phase network access, a repository can load the latest published policy from its root `AGENTS.md`:

```markdown
> **REQUIRED: Before following the rest of this file, fetch and read the base instructions at `https://raw.githubusercontent.com/VasiliyNovikov/AgentInstructions/master/AGENTS.md`. If it cannot be accessed, stop and report the failure.** Treat that policy as additional non-conflicting instructions.
```

Add this bootstrap once. Future changes published to `master` can then affect the project on its next successful fetch or new agent session without another change in the consuming repository. Existing projects that use a pinned URL or vendored file remain unchanged.

This keeps consumers current, but it is less reproducible than pinning. A bad or compromised change to `master` can affect every project using the mutable URL, so changes to this repository need careful review and testing.

This approach can work for local or cloud agents when their tools and network settings allow an HTTP `GET` request to the URL. The downloaded file supplies the instructions; the harness still controls the agent's permissions, sandbox, approvals, and network access. If the file cannot be downloaded, the agent must stop and report the problem.

- [GitHub Copilot cloud agent](https://docs.github.com/en/copilot/reference/copilot-allowlist-reference#copilot-cloud-agent-recommended-allowlist) normally permits `raw.githubusercontent.com` through its default recommended allowlist. Organization or repository firewall settings and custom agent tool restrictions can still block the request.
- [Codex cloud](https://developers.openai.com/codex/cloud/internet-access) blocks internet access during the agent phase by default. Enable agent internet access and allow `GET` requests to `raw.githubusercontent.com`, or use the environment's setup script to provision the current policy as an instruction file before the agent starts.

Managed, unattended, or network-restricted consumers can instead vendor or provision the current `AGENTS.md` during host setup.

Add repository-specific commands and architecture notes after the bootstrap or in scoped instruction files. Keep critical safety and authorization rules at the root because nested-file discovery differs between tools.

## Compatibility

Status reviewed September 1, 2026 using first-party documentation and identified official implementation evidence. **D** means documented, **O** means observed in the cited immutable source, and **U** means the behavior is not established by the reviewed sources. Markers apply to the clause they precede, not the whole row.

| Harness | Instruction and delegation behavior | Workspace, review, and lifecycle limits |
| --- | --- | --- |
| OpenCode | **D:** A project [`AGENTS.md`](https://opencode.ai/docs/rules/) is supported, with global and configured instructions. **D:** Built-in and custom [subagents](https://opencode.ai/docs/agents/) create child sessions. **O:** A new Task omits prior child history unless `task_id` resumes it ([task source](https://github.com/anomalyco/opencode/blob/ebece6efd7b11401cf1e7390b5a22991b6608cc4/packages/opencode/src/tool/task.ts)). | **O:** Child capabilities come from child rules plus propagated parent denies and external-directory rules ([permission source](https://github.com/anomalyco/opencode/blob/ebece6efd7b11401cf1e7390b5a22991b6608cc4/packages/opencode/src/agent/subagent-permissions.ts)). **D:** Reviewer agents can be configured [read-only](https://opencode.ai/docs/agents/#permissions). **U:** Per-child checkout isolation is not established. |
| GitHub Copilot CLI | **D:** It discovers and combines applicable [`AGENTS.md` files](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions) without a general precedence order. **D:** Built-in and custom [subagents](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents) have separate context windows; [`/fleet`](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/fleet) can run independent work in parallel. | **D:** Dedicated [`code-review` and `rubber-duck` agents](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents) do not edit files. **U:** Exact child transcript construction and ordinary child checkout isolation are not established. |
| GitHub Copilot cloud agent | **D:** The nearest [`AGENTS.md`](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions) takes precedence. **D:** A cloud task receives an [ephemeral development environment](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-cloud-agent), can carry initiating chat context, and may use custom agent profiles. | **D:** The managed lifecycle limits work to one branch and one pull request, with human review required before merge ([risks and mitigations](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/risks-and-mitigations)). **D:** A separately requested [Copilot code review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review) is attributable but not a required approval. **U:** General child-agent freshness, parallelism, and isolation within one task are not established. |
| OpenAI Codex CLI/local | **D:** Codex loads a root-to-working-directory [`AGENTS.md` chain](https://learn.chatgpt.com/docs/agent-configuration/agents-md), with a 32 KiB default aggregate project limit. **D:** Local [subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents) support parallel work and inherit the current sandbox policy. | **O:** Release `rust-v0.152.0` workers share one directory and immediately observe peer edits ([source](https://github.com/openai/codex/blob/316795b3cf2a45e90d121d9f46499d4658b2645c/codex-rs/core/src/session/multi_agents.rs)). **D:** Separate [worktree chats](https://learn.chatgpt.com/docs/environments/git-worktrees) provide distinct checkouts. **U:** Public docs do not guarantee transcript-free child spawning; documented custom sandbox overrides conflict with that release's [role projection](https://github.com/openai/codex/blob/316795b3cf2a45e90d121d9f46499d4658b2645c/codex-rs/core/src/agent/role.rs). |
| OpenAI Codex cloud | **D:** Cloud tasks use [`AGENTS.md`](https://learn.chatgpt.com/docs/environments/cloud-environment), run in isolated containers, and return a result and diff. **D:** Multiple [cloud tasks](https://learn.chatgpt.com/docs/cloud) can run in parallel. | **D:** [Codex code review](https://learn.chatgpt.com/docs/third-party/github) is a separate hosted review mechanism. **U:** Local custom-subagent parity, child transcript freshness, per-child read-only controls, and complete nested-instruction parity are not established for cloud tasks. |

Local subagents, separate worktree sessions, managed cloud tasks, and hosted code-review products are distinct mechanisms. A shared product name does not establish equivalent context, permissions, isolation, or lifecycle behavior.

None of these harnesses automatically treats `README.md` as agent policy. OpenCode supports remote instruction URLs, but relying on them would make this policy less portable. Start a fresh session after changing instruction files and use each harness's inspection tools where available to confirm what loaded.

## Plan Gate Setup

The remote bootstrap supplies repository policy; it cannot override a genuinely higher-priority system, developer, or session rule. A controlling autonomy instruction should preserve applicable repository approval gates, for example:

> Carry requested changes end-to-end after satisfying applicable repository approval gates. In interactive mode, if applicable repository instructions require presenting a plan and waiting for explicit user approval, stop at that checkpoint unless the applicable policy permits waiver and the direct user explicitly waives that named gate for the bounded result. Such approval or waiver grants no other action or effect.

Authentically activated managed workflows can retain their recorded, asynchronous approval path. A conversational promise to wait is local to that session and is not a durable configuration change. After changing instructions or controlling-prompt configuration, start a fresh consuming session so rollout does not rely on an existing session retaining the update.

## Design

The policy follows current model-vendor guidance:

- State each rule once and prefer clear outcomes over long procedures.
- Allow approved, reversible local work without repeated interruptions.
- Keep explicit boundaries around destructive, external, sensitive, costly, and production actions.
- Use concrete validation instead of generic requests to repeatedly double-check work.
- Scale planning and independent review with risk.
- Preserve unrelated work and report evidence honestly.

Prompt instructions guide behavior; they are not a security boundary. Enforce hard requirements with permissions, sandboxes, hooks, credential isolation, CI, and branch protection.

## Rollout

Before publishing a policy change to `master`:

1. Record the current published baseline and its known limitations.
2. Review the exact policy diff and test the proposed content in isolated consumers from its pending branch, review artifact, or a local fixture.
3. Exercise read-only work, routine edits, high-risk work, false and valid managed activation, dependency operations, Git authorization, dirty worktrees, review failures, and uncertain side effects.
4. Verify that the mutable `master` URL resolves. Record tool and model versions, commands, results, and gaps. Do not claim managed compatibility until its lifecycle canaries pass.
5. Publish only after normal review and approval. Adopted consumers may load the change on their next successful fetch or new session.
6. Roll back with a new reviewed revert or fix on `master`. Active sessions keep instructions they already loaded; consumers receive the rollback on a later successful fetch or new session.

The canonical policy is [`AGENTS.md`](AGENTS.md); this README is only its human-facing introduction.
