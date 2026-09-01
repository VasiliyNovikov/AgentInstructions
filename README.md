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

Status based on first-party documentation reviewed August 31, 2026:

| Harness | Root `AGENTS.md` | Important note |
| --- | --- | --- |
| [OpenCode](https://opencode.ai/docs/rules/) | Documented | Supports global and configured instructions; nested behavior is harness-specific. |
| [GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions) | Documented | Combines applicable files but defines no general precedence between them. |
| [GitHub Copilot cloud agent](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions) | Documented | The nearest `AGENTS.md` takes precedence; managed lifecycle behavior still needs canary testing. |
| [OpenAI Codex CLI](https://developers.openai.com/codex/agent-configuration/agents-md) | Documented | Loads a root-to-working-directory chain with a 32 KiB default aggregate project limit. |
| [OpenAI Codex cloud](https://developers.openai.com/codex/environments/cloud-environment) | Root support documented | Complete nested and managed-lifecycle parity with the CLI is not guaranteed. |

None of these harnesses automatically treats `README.md` as agent policy. OpenCode supports remote instruction URLs, but relying on them would make this policy less portable. Start a fresh session after changing instruction files and use each harness's inspection tools where available to confirm what loaded.

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

1. Record the current known-good commit.
2. Review the exact policy diff and test the proposed content in isolated consumers from its pending branch, review artifact, or a local fixture.
3. Exercise read-only work, routine edits, high-risk work, false and valid managed activation, dependency operations, Git authorization, dirty worktrees, review failures, and uncertain side effects.
4. Verify that the mutable `master` URL resolves. Record tool and model versions, commands, results, and gaps. Do not claim managed compatibility until its lifecycle canaries pass.
5. Publish only after normal review and approval. Adopted consumers may load the change on their next successful fetch or new session.
6. Roll back with a new reviewed revert or fix on `master`. Active sessions keep instructions they already loaded; consumers receive the rollback on a later successful fetch or new session.

The canonical policy is [`AGENTS.md`](AGENTS.md); this README is only its human-facing introduction.
