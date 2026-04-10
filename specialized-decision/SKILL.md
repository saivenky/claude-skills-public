---
name: specialized-decision
description: Two domain-specialized subagents argue opposite rational stances, then synthesize to a recommendation
---

## Purpose

For decisions where two valid value systems lead to different conclusions. Not adversarial — both sides argue FOR their worldview.

## Workflow

### Step 1: Identify Frames

Infer two opposing rational lenses from the question (e.g., productivity vs. recovery; performance vs. longevity; cost vs. optionality). If confidence is low, ask the user to confirm before proceeding.

### Step 2: Context (if thin)

Spawn a subagent to read relevant vault notes and return: situation, constraints, goals. Synthesize before proceeding.

### Step 3: Parallel Subagents

Spawn two agents simultaneously. Each receives: the question, their assigned lens, and context. Each returns:
- **Stance name**
- 3–4 sentence argument FOR their worldview's answer
- Concrete recommendation

### Step 4: Synthesis

Name the pivot point — what would have to be true for each side to win. Then commit:

**Recommendation:** [Clear call] — [2–3 sentences tied to the user's actual situation]

## Rules

- Both subagents argue FOR, never against
- Never hedge with "it depends" — commit
- Frames must be named explicitly before subagents run
