---
name: ship
description: "Ship the feature grilled or planned in this conversation — unattended, one slice at a time in its own worktree, landing on the default branch."
disable-model-invocation: true
---

# Ship

Read [`leading`](../leading/SKILL.md) before anything else and follow it; this file holds only the technical shipping steps. Invoking `/ship` is the EM leaving the room — the run is unattended.

`/implement` is user-invocable: read `~/.claude/skills/implement/SKILL.md` and follow it. Install it from [mattpocock/skills](https://github.com/mattpocock/skills).

## 1. Cut the slices

The feature is the one grilled or planned in **this conversation**. Its **vertical slices** come from its ticket files when they were written, else cut them from the conversation. Nothing in context to ship — say so in one line and stop.

Then claim the feature's **worktree** — one branch, one tree:

```
git worktree add <worktree-root>/<repo>-<slug> -b ship/<slug>
```

The **worktree root** — read from `CLAUDE.md`, else `../.worktrees/` beside the repo — sits outside every repo. Undeclared, use that fallback and record it in `CLAUDE.md` as part of the run. `<slug>` names the feature; that path is the working root for you and every subagent.

Scratch files — stubs, logs, screenshots, throwaway scripts — live in `.scratch/` inside the worktree, so removing the worktree removes them. List `.scratch/` in the repo's `info/exclude` if it is not there yet; `git rev-parse --git-common-dir` finds it, and every worktree shares it.

Announce the ordered slices and the model tier for each, and start.

## 2. Run the slices

Take slices in dependency order, **one at a time**, in the worktree. Each slice's brief adds to leading's: "Implement this slice with `/implement`. Land it **green**, then commit to the current branch. If you cannot go green, report why instead of committing. Put scratch files in the worktree's `.scratch/`, and list anything you left outside it."

An emulator, a device, a fixed port are **exclusive**: a slice holds one until it lands; a UI worktree takes its own port. Other sessions share them too — the repo's `CLAUDE.md`, or a skill it points to, names each one and how to lock it. When one is busy, the slice still builds, runs every check that does not need it, and reports the QA still pending. Hold that slice as **built, QA pending**, and once the resource frees, dispatch just the QA. QA is done only when it passes on that resource.

**Validate** every return — what reaches the EM must work as specified:

- run green in the worktree yourself
- check the diff against the acceptance criteria, and that tests exercise the behaviour rather than restate it — brief a subagent for this and take back a thin verdict; read the diff yourself only for taste work
- exercise it end-to-end when the slice is user-visible

When ticket files exist, mark the ticket `**Status:** landed — <sha>`, committed with the slice.

## 3. Land

When the last slice is green, **land** the branch:

1. Merge the **default branch** into `ship/<slug>` **inside the worktree**. It moved while you worked, so expect real conflicts; they are yours to resolve here, where a bad merge costs nothing.
2. Verify green again.
3. Land on the default branch as a strict fast-forward. Rejection means it moved again: re-merge, verify green, retry.
4. Remove the worktree.
5. Bounce whatever runs the code — restart the server, reinstall the app — so the EM can watch it live. The repo tells you how.

A blocked run leaves its worktree standing.

In the final report, "what stands between here and prod" names migrations, config, rollout order, and anything unverified.
