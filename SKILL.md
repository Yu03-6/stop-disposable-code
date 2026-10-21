---
name: stop-disposable-code
description: Keep code maintainable when modifying existing features, adding or extending functionality, fixing bugs, or refactoring a project. Ask the user whenever the requested change is unclear; never guess intended behavior. Before implementation, verify existing behavior, change boundaries and recovery, behavioral validation, durable project knowledge, and root-cause handling. Use especially for requests to change feature behavior. Exclude explanation-only questions and standalone prose or asset edits unless explicitly invoked.
---

# Stop Disposable Code

Make each requested change understandable, reversible, and verifiable so the project can continue evolving. Apply the five checks below before editing implementation code. This is an evidence-based self-check by the agent, not a request for the user to approve a checklist.

## Clarify uncertain requirements before acting

If there is any doubt about what the user wants changed, or any ambiguous description of scope, expected behavior, acceptance criteria, data handling, or compatibility, ask the user directly as soon as it is recognized. Do not choose a plausible interpretation and implement it.

- State the specific uncertainty and ask a concise, concrete question. Offer clearly distinguished options when useful, but do not treat a recommendation as the user's choice.
- Pause all implementation that depends on the answer. Do not build a guessed version, implement several speculative alternatives, or defer clarification until after making changes.
- Silence, elapsed time, an unanswered question, and a preselected option are not confirmation. Keep dependent work paused until the user answers.
- Continue only independent work that does not commit to an interpretation, such as reading code, tracing existing behavior, or preparing unaffected checks.
- Use explicit decisions already supplied in the conversation. Do not repeatedly ask answered questions. If earlier instructions conflict or remain unclear, ask which requirement should govern.
- Existing code establishes current behavior; it does not establish the user's desired new behavior. Do not use current conventions to silently resolve a genuine product ambiguity.
- Ordinary internal choices may follow established project conventions only when they preserve a clearly specified outcome. If the choice could change the requested scope or observable result and the user's intent is uncertain, ask first.
- Distinguish unclear intent from an unknown technical fact: inspect code or run focused diagnostics for technical facts; ask the user about intent. If an investigation exposes a requirement ambiguity, ask immediately.
- After an answer, briefly restate the resolved requirement and update the affected checks before editing. Apply this rule throughout the task, including when new ambiguity appears during implementation or validation.

## Before implementation: five required checks

Read applicable project instructions and inspect the relevant code and working-tree state. Gather only the evidence needed for the requested change. Read-only inspection and diagnostic commands are allowed before the checks are complete.

For each check, determine **ready**, **not applicable with a reason**, or **blocked with a concrete missing prerequisite**. Do not mark a check ready from assumptions about files that have not been inspected. A ready check means the current facts and the implementation approach are understood; it does not mean future tests have already passed.

### 1. Understand and reuse the existing implementation

- Trace the affected entry point through the relevant business logic, state ownership, data sources, and consumers. Inspect callers and existing tests where relevant.
- Identify where the rule or state is authoritative, what should be reused, and which current behavior must remain intact.
- Do not introduce a second source of truth, an alternate feature implementation, or duplicated business rules for convenience. If the existing structure is insufficient, identify the smallest necessary structural change.
- For a new project, explicitly record that no existing implementation exists and choose simple boundaries for the requested behavior.

### 2. Bound the change and establish recovery

- Define one coherent behavior change, its affected areas, and any necessary dependent changes. Preserve unrelated user edits.
- Inspect version-control status and available history when present. Identify how this change can be reviewed and rolled back without discarding pre-existing work.
- Use the existing version-control workflow. Make commits only when authorized or required by applicable project instructions; never automatically stage everything, reset user work, or push.
- If version control is absent, use a proportionate local recovery method for existing files being changed, or establish version control if authorized. New, isolated files may simply be removable. Do not block a safe small edit merely because a repository has no commits.
- Separate unrelated upgrades or redesigns from the task. Avoid speculative frameworks, compatibility layers, and abstractions for imagined future requirements.

### 3. Decide how to verify behavior and regressions

- Specify observable success criteria for the requested behavior and the existing behavior most likely to be affected.
- Select focused checks using the project's tools. For important business rules, cover relevant failure cases and state transitions as well as success; examples include payment cancellation, authorization failure, and persisted state after reload.
- Plan durable regression tests when the risk warrants them. Do not add tests that only repeat implementation details or impose a test framework for a trivial edit.
- Treat compilation, linting, and type checking as supporting evidence, not proof that a user flow works.
- Identify unavailable credentials, devices, services, or tools before promising runtime validation. Missing runtime access need not block implementation if correctness can be assessed sufficiently to proceed; explicitly retain the verification gap.

### 4. Keep essential project knowledge in the project

- Locate existing setup, configuration, architecture, and testing notes relevant to the change; verify critical instructions against actual project files.
- Determine whether this change needs a concise update covering how to run or test it, where its logic belongs, required configuration names, or an important limitation.
- Update the existing authoritative documentation when practical. If necessary knowledge exists only in the conversation and no suitable document exists, add the smallest useful project note.
- Do not require a new architecture document for every feature or block an unrelated edit on missing documentation. Mark this check not applicable when no durable knowledge changes.
- Never put secrets into project notes. Do not rely on chat history as the only explanation of a non-obvious design decision.

### 5. Address the cause and plan removal of replaced logic

- For a bug, identify the cause using a reproduction, relevant test, or supported code-path analysis. Distinguish evidence from hypotheses; if the cause is unknown, continue focused diagnosis rather than layering on speculative fixes.
- For a feature change, identify which existing rule is being extended or replaced and where that change belongs.
- Identify obsolete branches, duplicate state, and dead code created by the replacement. Remove them within the affected scope after checking consumers.
- Preserve fallbacks or compatibility behavior that have a demonstrated purpose. Do not use blanket exception swallowing, silent fallback data, or repeated special cases to conceal a broken data flow.

## Starting implementation

Before the first implementation edit, provide a short update in the user's language summarizing the findings of all five checks. Combine findings into a compact paragraph or list; cite the key files or evidence when helpful. Explain any not-applicable item and any unresolved limitation without dumping a long audit.

Proceed only when the intended change is clear and all checks relevant to the next change are ready or reasonably not applicable. Requirement ambiguity is a blocking prerequisite for dependent implementation: ask the user directly under the clarification rule above. Resolve technical unknowns through focused investigation where possible. Do not turn already resolved requirements into repeated approval requests.

Implement the bounded change using the existing project conventions. Revisit a check only when new evidence changes the scope, invalidates an assumption, or reveals a dependency. Do not repeatedly audit already settled items.

## Before completion

Close the loop on the same five checks:

1. Review the diff for reuse of authoritative rules and state, and for accidental parallel implementations.
2. Confirm the diff stays within scope, preserves unrelated work, and has the identified recovery path.
3. Execute the selected validation. Fix failures caused by this change; distinguish pre-existing failures and unavailable checks. Never report planned or unexecuted tests as passed.
4. Make the necessary documentation updates, checking paths and commands for consistency.
5. Verify replaced logic is removed or retained for a specific reason, and that the fix addresses the supported cause.

Report the resulting behavior, actual validation, and material remaining gaps concisely. If checks fail or cannot run, describe the limitation accurately rather than declaring full verification. Stop when the requested work and appropriate verification are complete.
