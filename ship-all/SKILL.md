---
name: ship-all
description: "Ship every ready feature in this repo at once — one tech lead, one worktree per feature, landing in completion order."
disable-model-invocation: true
---

# Ship All

Ship every ready feature in this repo. You are the tech lead running **all** of it; the user is your **EM**.

Read `~/.claude/skills/ship/SKILL.md` and follow it per feature — worktree, slice briefs, validation, status write-back, landing. Only the coordination deltas live here.

## 1. Pool the features

Discover every feature in this repo by ship's rule: a ticket directory whose tickets carry `**Status:** ready-for-agent`. One repo only — a second repo is a second run.

Present the pool once, ranked by closeness to done: ready count, total open, and any **exclusive** resource two features both need (emulator, device, port). Note any ticket blocked by a ticket in another feature. The EM confirms the set — the last check-in until a blocker or the final report.

## 2. Run every frontier

Each feature gets its own worktree and `ship/<slug>` branch. Within a feature, slices run **one at a time** in dependency order; across features they run in parallel, so several slice subagents are in flight at once.

Work the pooled frontier breadth-first: every feature with an unblocked ready ticket is started, no stagger. Contention is resolved by exclusive resources — a feature needing one that's held waits for it — not by a queue.

You spawn slice subagents directly, sized to the slice as `ship` §2 says. There is **no per-feature lead**: one tech lead, many engineers.

## 3. Validate every return yourself

Ship's validation is the bar, per slice, no exceptions when the pool is busy: run green in that worktree, read the diff against the acceptance criteria, check the tests exercise the behaviour, exercise it end-to-end when it's user-visible. Delegate legwork to a subagent when that's cheaper; the verdict is yours.

Red is never escalation. Diagnose and repair, in whichever feature it lands in, before that feature moves on.

## 4. Land in completion order

A feature lands the moment **its own** frontier empties — no waiting for the pool. Run ship's landing steps in its worktree, then bounce whatever runs the code so the EM can watch the feature live.

The default branch moves under the other features as you land. Re-merging it into every standing branch is yours: resolve the conflicts in the worktree, or hand a hairy one to a subagent with the context it needs — the resolution stays your call.

## 5. Report as to an EM

- **A blocker** goes up immediately: the decision, the options, your recommendation, what's parked. Park that feature, keep the rest moving.
- **A feature landing is not an interrupt.** Land it, log it, keep going.

When the pool empties, one consolidated report — per feature: slices landed with commits, judgement calls made alone, anything parked, and what stands between here and prod.

Then stop and wait for the EM.
