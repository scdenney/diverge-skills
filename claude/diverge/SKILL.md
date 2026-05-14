---
name: diverge
version: 1.0.0
description: |
  Before implementing, generate 3–5 conceptually distinct approaches
  labeled by creativity dimension (Novel, Surprising, Diverse, Conventional).
  Holds for user selection before touching any code or files.
  Based on Creative Preference Optimization (Ismayilzada et al., 2025) —
  brainstorm-then-select for maximizing novelty, surprise, and diversity.
license: CC-BY-4.0
compatibility: claude-code
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - AskUserQuestion
---

# Diverge

Interrupt the default path of jumping to the most probable — and least creative — solution.

## When to invoke

Use `/diverge <task>` when:
- multiple non-obvious implementations exist
- you want to avoid the conventional approach
- the task is creative, architectural, or analytical, not purely mechanical

Do not use for rote tasks with one correct answer (e.g., fix this syntax error).

## Behavior

Given `$ARGUMENTS`:

### Step 1 — Clarify if needed

If the task is ambiguous about what "good" looks like, ask **one** focused question before proceeding. Skip this if the goal is clear. Do not ask about implementation details.

### Step 2 — Generate approaches

Produce **3–5 approaches** that are genuinely conceptually distinct. Differences must be in underlying mechanism, not surface vocabulary.

Label each with its primary creativity dimension:

- **[Novel]** — semantically far from the conventional solution; different conceptual basis
- **[Surprising]** — violates the obvious assumption about how this should work; would not be the first answer
- **[Diverse]** — maximally different from the other approaches in this list
- **[Conventional]** — the expected path, included as a reference point

For each approach provide:
1. Core mechanism — one sentence naming the key insight
2. How it works — two to three sentences on the mechanism and what makes it distinct
3. Main tradeoff — one sentence

### Step 3 — Hold

Do not implement. Present all approaches, then ask:

> "Which approach should I pursue? Or should I synthesize elements from multiple?"

## Constraints

- At least one approach must be **[Surprising]**
- At least one must be **[Novel]**
- Approaches must not merely restate each other with different vocabulary
- Quality matters second in this phase — novelty and surprise come first
- Do not use markdown headers per approach — keep the list scannable

## After selection

Implement the selected approach directly. If the user asks to synthesize, identify which elements are mechanically compatible and propose a brief hybrid plan before implementing.
