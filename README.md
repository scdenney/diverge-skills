# diverge-skills

Creative divergence skills for Claude Code and Codex. Based on [Creative Preference Optimization](https://arxiv.org/abs/2505.14442) (Ismayilzada et al., EMNLP 2025): before implementing, generate multiple conceptually distinct approaches — labeled by creativity dimension — and select.

---

## Background

### What the paper did

Ismayilzada et al. proposed **Creative Preference Optimization (CrPO)**, a modification to the standard preference alignment training used in models like Claude and GPT-4o. Standard alignment (RLHF/DPO) optimizes for human-preferred outputs — which in practice means the most expected, least surprising ones. CrPO injects four additional signals into that training objective:

- **Novelty** — how semantically distant a response is from other known responses (measured via divergent semantic integration)
- **Diversity** — pairwise semantic distance from other responses to the same prompt
- **Surprise** — negative log-likelihood under a reference model (how unexpected the output is)
- **Quality** — reward model score

Each signal is weighted independently, letting you tune which creativity dimensions to emphasize.

They trained and evaluated models using **MUCE**, a new dataset of over 200,000 human responses and creativity ratings across 30+ psychological creativity assessments.

### What they found

- Standard preference alignment actively reduces creativity. Models converge on high-quality but low-novelty, low-surprise outputs. Claude 3.7 Sonnet and GPT-4o are explicitly tested as baselines and cluster in this region.
- Small models (7–8B parameters) fine-tuned with CrPO outperform GPT-4o and Claude 3.7 on human creativity judgments.
- The winning variant — **CrPO-nov-div-sur** (novelty + diversity + surprise, no quality injection) — had the highest win rate in head-to-head human evaluation on creativity.
- Adding a quality signal recovers some quality but reduces creativity, confirming a real tradeoff.
- Prompting strategies (their "Brainstorm, then select" baseline) and decoding strategies (min-p sampling) both improved creativity over the base model — but CrPO models with standard prompting still beat them.

### Implications

The core problem is structural: any model trained with standard alignment will tend toward its most probable output, which is by definition the least surprising one. Fine-tuning on creativity signals fixes this at the weight level, but that isn't accessible for closed models like Claude or Codex.

However, the paper's own prompting baseline — **brainstorm-then-select** — showed meaningful gains and is fully accessible via prompt engineering. The key mechanics are:

1. Generate multiple approaches before committing to any
2. Require that approaches differ in underlying mechanism, not just vocabulary
3. Require at least one approach that is explicitly surprising or non-obvious
4. Defer quality and implementation to after selection

These mechanics don't require fine-tuning. They require enforcing a structure that the model won't enforce on its own.

### What these skills do about it

These skills encode the brainstorm-then-select framework as invocable behavior for Claude Code and Codex. When you invoke `/diverge` or `/diverge-codex`, the model is required to:

- Generate 3–5 approaches labeled by creativity dimension before touching any code or files
- Include at least one [Surprising] and one [Novel] approach
- Hold for your selection before implementing

This doesn't replicate CrPO's training-time creativity injection, but it applies the most accessible version of its core insight: forcing divergence before convergence produces more novel outputs than asking for a single solution.

---

## Skills

### `/diverge` — Claude Code

Generates 3–5 approaches before implementing anything. Labels each as [Novel], [Surprising], [Diverse], or [Conventional]. Holds for user selection.

**Install:**
```bash
ln -s /path/to/diverge-skills/claude/diverge ~/.claude/skills/diverge
```

**Use:** `/diverge <your task>`

---

### `/diverge-codex` — Claude Code → Codex

Delegates the brainstorm phase to Codex (GPT-5.4). Claude orchestrates; Codex generates approaches; Claude presents them for selection; Codex implements the chosen one.

**Install:**
```bash
ln -s /path/to/diverge-skills/claude/diverge-codex ~/.claude/skills/diverge-codex
```

**Use:** `/diverge-codex <your task>`

---

### `$diverge` — Codex native

The same diverge behavior as a native Codex skill. Invoke inside Codex directly.

**Install:**
```bash
ln -s /path/to/diverge-skills/codex/diverge ~/.codex/skills/diverge
```

**Use:** Invoke with `$diverge` inside Codex, or reference in a Codex task.

---

## Creativity dimensions

| Label | Meaning |
|---|---|
| [Novel] | Semantically far from the conventional solution; different conceptual basis |
| [Surprising] | Violates the obvious assumption; not the first answer |
| [Diverse] | Maximally different from the other options in the list |
| [Conventional] | The expected path, included for contrast |

At least one approach is always [Surprising] and one is always [Novel].

---

## Planned

- `creative-review` — post-implementation audit that asks whether a less obvious approach existed

---

## Reference

Ismayilzada, M., Laverghetta Jr., A., Luchini, S. A., Patel, R., Bosselut, A., van der Plas, L., & Beaty, R. E. (2025). [Creative Preference Optimization](https://arxiv.org/abs/2505.14442). *Findings of the Association for Computational Linguistics: EMNLP 2025*, 9580–9609.
