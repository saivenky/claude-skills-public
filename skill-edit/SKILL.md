---
name: skill-edit
description: Word-budgeted editor for skill files — new skills ≤500 words, edits never grow.
disable-model-invocation: true
---

# Skill Editor

## When to Use
Invoke by hand when you want the word budget enforced on a skill file. Nothing fires it automatically.

---

## Edit Mode

1. Read target skill file; run `wc -w` for baseline
2. Apply all requested changes
3. Run `wc -w` on draft
4. **If draft > baseline:** condense — trim phrasing, merge sentences. Keep all information. Recount; repeat until ≤ baseline
5. Show `Word count: {baseline} → {final}`, confirm, save

---

## Create Mode

1. Draft skill content
2. Count words
3. **If draft > 500:** condense. Recount; repeat until ≤500
4. Show `Word count: {final} / 500`, confirm, create folder + SKILL.md

---

## Rules

- New skills: ≤500 words.
- Edits: final count ≤ baseline. Add first, then condense to fit.
- Show word count before saving.
- Don't over-prescribe — a rule that solves the current need may constrain unexpectedly.
