---
name: decision
description: Structured decision-making — surface the goal, make an initial call, stress-test with two adversarial subagents, then render a final verdict
---

## Purpose

Make a clear, reasoned decision from context — no user input during the process. Works for personal and technical decisions.

## Workflow

### Step 0: Assess Context Depth

If the conversation is thin (topic stated in a single message with no elaboration, constraints, or history), spawn a context-gathering subagent. It reads relevant files — vault notes, project docs, recent daily notes, code — and returns: goals at stake, constraints, and what's been tried or ruled out. Synthesize before proceeding.

### Step 1: Surface the Goal

State what is being chosen between, what outcome is being optimized for, and what constraints apply. If ambiguous, infer the most plausible interpretation — do not ask.

### Step 2: Initial Decision

Commit to a clear, specific decision:
- **Decision:** One sentence
- **Reasoning:** 2–3 sentences explaining why

### Step 3: Adversarial Review

Spawn two subagents in parallel:

**Steelman:** Build the strongest case FOR the initial decision — evidence, analogies, logic. Ignore counterarguments.

**Red Team:** Build the strongest case AGAINST it. Assume it's wrong; find the best alternative and surface risks, overlooked factors, failure modes.

Each returns: a 2–3 sentence argument and, if overturning, an alternative decision with reasoning.

### Step 4: Weigh

- Red Team found no flaw Steelman can't address → affirm
- Red Team found a genuine flaw → change the decision
- State explicitly what shifted (or didn't) and why

### Step 5: Final Verdict

- **Final decision:** One clear sentence
- **Confidence:** High / Medium / Low
- **Key risk:** One sentence on what to watch for

## Guardrails

- No user input during the process
- Never hedge with "it depends" — commit to a position
- Steelman must genuinely argue FOR; Red Team must genuinely argue AGAINST — not perform balance
