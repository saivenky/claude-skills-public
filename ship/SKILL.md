---
name: ship
description: "Drive a feature's tickets to done — one slice at a time, each in a fresh subagent, escalating only what an EM would decide."
disable-model-invocation: true
---

# Ship

Drive the work to done. You are the tech lead; the user is your **EM**.

`/to-tickets` and `/implement` are user-invocable: read `~/.claude/skills/<name>/SKILL.md` and follow it. Install both from [mattpocock/skills](https://github.com/mattpocock/skills).

## 1. Set the frontier

Collect the run's **vertical slices**.

- **Given a target** — the tickets, plan, or conversation in context.
- **Given nothing** — discover them. A feature is a ticket directory (`.scratch/<slug>/issues/`, or the configured tracker); a ticket is **ready** when its status is `ready-for-agent` and every ticket blocking it is done. Rank features by closeness to done, with ready counts; the EM picks one, even when it's the only candidate.
- **Neither** — run `/to-tickets` first.

Then claim the run's **worktree** — a run owns one branch and one tree:

```
git worktree add <worktree-root>/<repo>-<slug> -b ship/<slug>
```

The **worktree root** — read from `CLAUDE.md`, else ask the EM and offer to record it there — sits outside every repo. `<slug>` names the feature; that path is the working root for you and every subagent.

Show the ordered slices and confirm once — the last check-in until ship.

## 2. Run the frontier

Take slices in dependency order, **one at a time**. For each, spawn a subagent with a fresh context whose brief is:

- the worktree path, absolute, as its working root — pass it to every command; a `cd` won't survive the next call
- the slice's behaviour and acceptance criteria, plus surrounding decisions (domain vocabulary, ADRs, prior slices' shape)
- "Implement this slice with `/implement`. Land it **green** — typecheck and tests pass — then commit to the current branch. If you cannot go green, report why instead of committing."

**Size the model to the slice.** Opus when the work turns on **taste** — prompt text, UI, copy, anything a passing test can't vouch for. Sonnet otherwise; Haiku where the slice is mechanical. Log the choice with the slice.

An emulator, a device, a fixed port can't be shared. Treat each as **exclusive**: a slice holds it until it lands; a UI worktree takes its own port. Log the claim for parallel runs.

When a subagent returns, **validate it yourself** — your job rides on what reaches the EM working as ticketed:

- run green in the worktree; never take the claim on trust
- read the diff against the acceptance criteria; check tests exercise the behaviour, not restate it
- exercise it end-to-end when the slice is user-visible

Hand parts to a subagent when cheaper; the verdict is yours. Red is never escalation — repair it yourself, or re-spawn the slice a tier up; either way it stays with you. Then mark the ticket `**Status:** landed — <sha>`, committed with the slice, so discovery sees it done.

Log slices landed and every judgement call made alone.

## 3. Report as to an EM

Your EM hired you to decide: between slices, decide and keep moving — log the call rather than asking.

Surface work at exactly two moments:

- **A blocker** — a live decision only the EM can make: it changes *what* is being built rather than *how*, costs more than a slice to reverse, or reaches outside the repo (spend, external services, anything user-facing in prod). A failure exposing a broken spec is a blocker; the failure itself never is.
- **Ready to ship** — the frontier is empty.

State a blocker as: the decision, the options, your recommendation, what's parked. Then take the next unblocked slice.

## 4. Ship

When the frontier empties, **land** the branch:

1. Merge the **default branch** into `ship/<slug>` **inside the worktree**. It moved while you worked — others commit to it too — so expect real conflicts. They resolve here, where a bad merge costs nothing and the context is yours; a conflict is yours to resolve, never a blocker.
2. Verify green again.
3. Land on the default branch as a strict fast-forward. Rejection means it moved again: re-merge, verify green, retry.
4. Remove the worktree.
5. Bounce whatever runs the code — restart the server, reinstall the app — so the EM can watch it live. The repo tells you how.

A blocked run leaves its worktree standing.

Then report:

- slices landed, with their commits
- judgement calls made alone, for the EM to overturn
- what stands between here and prod — migrations, config, rollout order, anything unverified

Then stop and wait for the EM.
