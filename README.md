# Stop Disposable Code

A Codex skill for maintaining and extending AI-assisted codebases without accumulating duplicate rules, speculative patches, and undocumented behavior.

**Clarify the intended change first. Check maintainability before editing. Verify the result before finishing.**

## Why this skill exists

A feature can work today while making tomorrow's change unnecessarily difficult. This skill makes the agent inspect the existing implementation, preserve a recovery path, plan meaningful validation, keep essential knowledge in the project, and address causes rather than stack patches.

It is designed especially for requests to modify existing functionality, extend features, fix bugs, and refactor code. Explanation-only questions and standalone prose or asset edits are outside its normal automatic selection scope.

## Ask instead of guessing

When the agent has doubts about the requested change or finds ambiguous requirements, it must ask the user directly before implementing anything dependent on the answer.

For example, “Keep users signed in longer” does not specify the session duration or whether the change applies to all devices. The agent should ask about those requirements instead of choosing a duration and modifying authentication behavior.

The agent must:

- Identify the specific uncertainty and ask a concise question.
- Wait for an answer before making dependent changes.
- Treat neither silence nor a suggested default as consent.
- Continue only work that is independent of the unresolved interpretation.
- Reuse explicit decisions already provided instead of asking repeatedly.
- Ask again if new requirement ambiguity appears later in the task.

Technical facts should be investigated in the project. Desired product behavior should be clarified with the user. Established conventions can guide internal implementation choices when the requested outcome is already clear.

## Five checks before implementation

| Check | What the agent establishes |
| --- | --- |
| Existing implementation | The relevant code path, authoritative business rules and state, reusable logic, and behavior to preserve. |
| Scope and recovery | A coherent change boundary, protection for unrelated edits, and a proportionate way to review and roll back changes. |
| Behavioral validation | Observable success criteria, relevant regression checks, and any unavailable runtime prerequisites. |
| Durable project knowledge | Which existing setup, configuration, or design notes need updating so future work does not depend on chat history. |
| Root cause and cleanup | Evidence for the cause of a bug, or the rule a feature replaces, plus obsolete logic that should be removed. |

Each check is marked ready, not applicable with a reason, or blocked by a concrete prerequisite. Before editing implementation code, the agent briefly reports the findings. This is the agent's evidence-based self-check; it does not require the user to approve five checklist items.

A ready validation plan does not mean tests have passed. Actual checks run after implementation, and the completion report must distinguish passed, failed, and unexecuted verification.

## Install

Ask Codex to use its skill installer:

```text
Use $skill-installer to install the skill from https://github.com/Yu03-6/stop-disposable-code (the skill is at the repository root).
```

For manual installation, copy `SKILL.md` and the `agents/` directory together into a `stop-disposable-code` folder in a skill location supported by your Codex environment. See the [official Codex skills documentation](https://developers.openai.com/codex/skills/) for current discovery and installation details.

## Use

Explicit invocation:

```text
Use $stop-disposable-code to modify the subscription cancellation flow.
Ask me about any unclear requirements before changing dependent code.
```

Automatic selection is enabled in `agents/openai.yaml`. Its description targets feature changes, extensions, bug fixes, and refactoring. Automatic selection is model-driven and is not guaranteed for every matching request; invoke the skill explicitly when you want to ensure it is selected.

The instructions are written in English. Questions and progress reports should use the user's language.

## What this skill does not require

- A rewrite, elaborate architecture, or abstractions for hypothetical future features.
- A new testing framework or architecture document for every small change.
- Automatic commits, pushes, resets, or staging of unrelated work.
- Repeated approval of clear requirements or an audit of the entire repository.

It respects applicable project instructions and the user's authorized scope. It is an instruction workflow, not a static analyzer or a guarantee of bug-free code.

## Repository contents

```text
stop-disposable-code/
├── SKILL.md             # Maintainability and clarification workflow
├── agents/
│   └── openai.yaml      # Display metadata and automatic-selection policy
└── README.md            # Overview, installation, and usage
```

The skill is self-contained and requires no custom scripts or additional tools. Its implementation and verification steps use the project's existing tools.
