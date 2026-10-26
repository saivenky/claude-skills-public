# Slicing

How many slices a feature gets, and where to cut them. Both `/to-tickets` and `/ship` cut by this file. The cutter decides the size alone — no one is asked — and records the call so the user can overturn it.

## 1. Rate the feature

Rate the feature **small**, **medium**, or **large** by how many modules or layers it crosses and how many new concepts it introduces. Check the rating against the estimated diff size; if they disagree, look again before moving on.

The rating sets the slice **budget**:

| Rating | Budget |
| ------ | ------ |
| small  | 1      |
| medium | 2–3    |
| large  | 4–6    |

These numbers are provisional — tune them after the next few runs.

## 2. Cut within the budget

- Cut one slice per user-visible **behaviour**. Each slice carries the docs and tests for its behaviour, so every slice is a behaviour.
- More behaviours than budget → group related behaviours into one slice until the count fits. The budget wins.
- A prefactor lands inside the slice that needs it, as that slice's first step, and counts toward the budget like any other work in it.

## 3. Respect the ceiling

The fresh context window is the **ceiling**, and it beats the budget. A slice estimated at over ~800 lines of diff or ~10 files is too big: split it, go over budget, and state why.

## 4. Record the call

Write one line naming the rating, the budget, the slice count, and the reason for any overage:

```
Sizing: medium — budget 2–3, cut 3.
Sizing: large — budget 4–6, cut 7; the sync slice crossed the ceiling (~1,200 lines).
```

Put it where the user sees the breakdown: the ticket index when tickets are written, the announcement when `/ship` cuts from the conversation.

The cut is done when the line is recorded, the count is within budget or the overage is explained, and every slice is a behaviour.
