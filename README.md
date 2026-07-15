# AgentInstructions

A tool-agnostic `AGENTS.md` baseline for AI coding assistants. It is designed to be shared across repositories and layered with repository- and directory-specific instructions.

The baseline emphasizes:

- minimal, scoped changes that preserve unrelated work
- quality-first context gathering, implementation, and validation
- proportional planning with independent fresh-context review
- user approval before implementation and after reviewing the resulting diff
- explicit protection around destructive, external, and Git operations

Keep universal working agreements in this repository. Put stack-specific commands, architecture, CI details, and conventions in each repository's `AGENTS.md`. Put conditional or specialized workflows in skills, hooks, subagents, or tool configuration so they do not burden every task.
