---
name: diverge-codex
version: 1.0.0
description: |
  Delegate a task to Codex (GPT-5.4) with creative divergence instructions.
  Claude orchestrates; Codex generates 3–5 distinct approaches before
  implementing. Based on Creative Preference Optimization (Ismayilzada et al.,
  2025) — brainstorm-then-select adapted for Codex delegation.
license: CC-BY-4.0
compatibility: claude-code
allowed-tools:
  - Read
  - Agent
---

# Diverge → Codex

Delegate a task to Codex with explicit creative divergence instructions. Claude structures the prompt; Codex brainstorms; Claude presents options for selection; Codex implements the chosen approach.

## Steps

1. **Take the task.** Use `$ARGUMENTS`. If empty, ask the user what they want to solve.

2. **Clarify if needed.** If the task is ambiguous about the goal (not the implementation), ask one focused question before delegating. Skip if clear.

3. **Compose the brainstorm prompt.** Use the XML template below, replacing `TASK` with the user's request verbatim.

4. **Spawn `codex:codex-rescue`** with the composed prompt. Codex will generate approaches and stop without implementing.

5. **Present approaches** to the user. Ask which to pursue, or whether to synthesize.

6. **Compose the implementation prompt.** Use the follow-up template below, replacing `TASK` and `SELECTED_APPROACH`.

7. **Spawn `codex:codex-rescue`** again with the implementation prompt.

## Brainstorm prompt template

```xml
<task>
Before implementing, generate 3–5 conceptually distinct approaches to:

TASK

Label each approach:
  [Novel]       — different conceptual basis from the conventional solution
  [Surprising]  — violates the obvious assumption; not the first answer
  [Diverse]     — maximally different from the other options in this list
  [Conventional]— the expected path, for contrast

For each approach provide:
- Core mechanism (1 sentence)
- How it works and what makes it distinct (2–3 sentences)
- Main tradeoff (1 sentence)

Do not implement. Present all approaches and stop.
</task>

<constraints>
At least one approach must be [Surprising].
At least one must be [Novel].
Approaches must differ in underlying mechanism, not just vocabulary.
Prioritize novelty and surprise over immediate quality.
Do not write any implementation code.
</constraints>

<structured_output_contract>
Numbered list only. No preamble. No implementation code.
Format each: number, label, mechanism line, explanation, tradeoff line.
</structured_output_contract>
```

## Implementation prompt template

```xml
<task>
Implement the following approach to: TASK

Selected approach: SELECTED_APPROACH

Implement it fully. Edit files in place where applicable.
</task>
```

## Notes

- Always brainstorm first — never ask Codex to implement on the first delegation
- Do not inject your own implementation preferences — let Codex generate the approaches
- Present Codex's approaches verbatim; do not paraphrase or filter them
