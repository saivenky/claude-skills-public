---
name: ship
description: "Drive a plan or ticket set to done — one slice at a time, each in a fresh Opus subagent, escalating only what an EM would decide."
disable-model-invocation: true
---

# Ship

Drive the work to done. You are the tech lead running the build; the user is your **EM**.

Ship hands off to `/to-tickets` and `/implement`, not in this repo — install from [mattpocock/skills](https://github.com/mattpocock/skills). Both are user-invocable only — to run one, read `~/.claude/skills/<name>/SKILL.md` and follow it.

## 1. Set the frontier

Collect the **vertical slices** from the tickets, plan, or conversation already in context. If none exist, run `/to-tickets` first.

Then claim the run's **worktree**. A run owns one branch and one tree, so concurrent runs never share a working directory:

```
git worktree add <worktree-root>/<repo>-<slug> -b ship/<slug>
```

The **worktree root** is where this machine keeps agent worktrees — a declared fact, not a guess. Read it from `CLAUDE.md`; if none declares one, ask the user and offer to record the answer there. Keep it outside every repo, or it turns up in project discovery and recursive greps.

`<slug>` names the feature. That absolute path is the run's working root — you and every subagent operate there.

Show the ordered slice list and confirm it once. This is the last check-in until ship.

## 2. Run the frontier

Take slices in dependency order, **one at a time**. For each, spawn a subagent (`model: opus`) with a fresh context whose brief is:

- the worktree path, absolute, as its working root — pass it to every command; a `cd` won't survive the next call
- the slice's behaviour and acceptance criteria
- the surrounding decisions it needs (domain vocabulary, ADRs, prior slices' shape)
- "Implement this slice with `/implement`. Land it **green** — typecheck and tests pass — then commit to the current branch. If you cannot go green, report why instead of committing."

Inside the worktree "the current branch" is `ship/<slug>`, so the commit lands where it belongs.

Some things can't be shared between runs: an emulator, a physical device, a fixed port. Treat each as **exclusive** — a slice needing one holds it until that slice lands, and a worktree serving a UI takes its own port instead of the default. Note the claim in the log so a parallel run can wait for it.

When a subagent returns: verify green yourself, then start the next slice. A slice that returns red is yours to diagnose — repair it or escalate it.

Keep a running log of slices landed and every judgement call you made alone.

## 3. Report as to an EM

Your EM has hired you to decide. Between slices, decide and keep moving — record the call in the log rather than asking.

Surface work in progress at exactly two moments:

- **A blocker** — a live decision only the EM can make: it changes *what* is being built rather than *how*, costs more than a slice to reverse, or reaches outside the repo (spend, external services, anything user-facing in prod).
- **Ready to ship** — the frontier is empty.

State a blocker as: the decision, the options, your recommendation, and what is parked until it is answered. Then park that slice and take the next unblocked one.

## 4. Ship

When the frontier empties, **land** the branch:

1. Merge the **default branch** into `ship/<slug>` **inside the worktree**. Every conflict is resolved here, in isolation, where a bad merge costs nothing and you can retry freely — you hold the context for these calls, which is why the resolution is yours and not the EM's.
2. Verify green again.
3. Land on the default branch as a strict fast-forward. Rejection means it moved while you worked: re-merge and retry.
4. Remove the worktree.

A run that ends red or blocked leaves its worktree standing, so the wreckage can be inspected.

Then report:

- slices landed, with their commits
- judgement calls made alone, so the EM can overturn any of them
- what stands between here and prod — migrations, config, rollout order, anything unverified

Then stop and wait for the EM.
