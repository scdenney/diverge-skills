# diverge-skills

Creative divergence skills for Claude Code and Codex. Based on Creative Preference Optimization (Ismayilzada et al., EMNLP 2025): before implementing, generate multiple conceptually distinct approaches — labeled by creativity dimension — and select.

Standard preference alignment pushes models toward the most probable (least surprising) output. These skills interrupt that default by enforcing a brainstorm-then-select phase.

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

## Planned

- `creative-review` — post-implementation audit that asks whether a less obvious approach existed

## Reference

Ismayilzada et al. (2025). [Creative Preference Optimization](https://arxiv.org/abs/2505.14442). *Findings of EMNLP 2025*.
