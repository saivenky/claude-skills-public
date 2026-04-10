---
name: roi-prioritize
description: Prioritize tasks in daily notes using ROI reasoning, handle task rollovers from previous days, and convert to actionable checkboxes
---

# ROI Task Prioritization

## Overview

This skill prioritizes tasks in daily notes based on Return on Investment (ROI) reasoning. It handles rollover tasks from previous days, uses judgment-based comparison to rank tasks, and converts them to prioritized checkboxes.

**Core workflow:**
1. Read today's and yesterday's daily notes
2. Detect and handle task rollovers (manual or assisted)
3. Prioritize all tasks using ROI reasoning
4. Update today's note with prioritized, checkbox-formatted tasks

## When to Use

- When you have unprioritized tasks in today's `## Todo` section
- When you want to roll over tasks from yesterday
- When you need help deciding which tasks to focus on
- Daily (typically morning) to organize your task list

## ROI Framework

**ROI = Identity Compound Yield / Investment** — use judgment, not formulas.

**Return — Identity reasoning:**
1. What persona regularly does this task? (Every task maps to one — no exceptions)
2. What other traits/habits does that persona carry?
3. What does strengthening this identity yield over years of repetition?

**Investment:**
- **Time**: estimated minutes/hours
- **Energy**: mental or physical difficulty (trivial / low / moderate / high)

**If effort is genuinely unknown** (range too wide to judge ROI, e.g. "fix printer" = 5 min or 2 hrs):
ask the user before ranking. Batch all uncertain tasks into one question.

## Core Workflow

### Step 1: Read Daily Notes

- Today: `Daily/YYYY-MM-DD.md` (use current date)
- Yesterday: `Daily/YYYY-MM-(DD-1).md` (calculate previous date correctly)
- Handle month/year boundaries (e.g., 2025-01-01 → yesterday is 2024-12-31)

### Step 2: Detect and Handle Rollovers

**Find potential rollovers:**
1. Extract uncompleted tasks from yesterday's `## Todo` section (tasks without `[x]`)
2. Extract tasks from today's `## Todo` section
3. Use fuzzy matching to find tasks in today that might have been manually copied from yesterday:
   - Case-insensitive comparison
   - Ignore existing `(from MM-DD)` annotations
   - Allow minor differences (typos, extra spaces, slight rewording)
   - Match based on substantial similarity (core intent is the same)

**Present to user:**
- Show detected manual rollovers: "Found in both today and yesterday"
- Show remaining yesterday tasks: "Available to roll over from yesterday"
- Ask user which tasks should officially roll over (can select from both lists)

**Process rollovers:**
1. Add `(from MM-DD)` annotation to selected tasks in today's note:
   - **CRITICAL**: If a task already has a `(from MM-DD)` annotation, preserve the original date
   - Only add new annotations for tasks that don't have one (first-time rollovers)
   - Example: Task with `(from 01-15)` rolled from 01-16 to 01-17 stays as `(from 01-15)`
2. Remove rolled-over tasks from yesterday's `## Todo` section
3. Keep other sections in yesterday's note unchanged

### Step 3: Prioritize Using ROI Reasoning

**Before ranking — check for unknown effort:**
- Scan all tasks for genuinely ambiguous effort (range too wide to judge ROI)
- If any found, ask the user in a single batched question, then proceed

**For each task:**
1. Name the persona that regularly does this task
2. Describe what that identity's ecosystem compounds into over years
3. Estimate time + energy investment
4. Rank by compound yield / investment

Note: high-compound identities with trivial investment can outrank high-compound identities with heavy investment.

### Step 4: Update Today's Note

**Format prioritized tasks:**
- Convert from bullets `- Task name` to checkboxes `- [ ] Task name`
- **Preserve task meaning exactly** - do NOT condense, abbreviate, or remove context from task descriptions
- CAN fix typos, spelling, and capitalization for consistent formatting
- Reorder by priority (highest ROI first)
- Annotation format: `— Persona: compound yield | time, energy`
- Include `(from MM-DD)` for rolled-over tasks before the annotation

**Update `## Todo` section:**
- Replace the entire `## Todo` section with prioritized tasks
- Preserve other sections in the daily note (YAML, other headings, etc.)
- If `## Todo` doesn't exist, create it

## Task Format Reference

**Unprioritized (before skill runs):**
```markdown
## Todo
- Fix leaky faucet
- Call insurance about bill
- Draft bike listing
```

**Prioritized (after skill runs):**
```markdown
## Todo
- [ ] Call insurance about bill — Financially Responsible: protects household resources | 15 min, low
- [ ] Draft bike listing — Seller/Declutterer: income + lighter home compounds over time | 30 min, low
- [ ] Fix leaky faucet (from 12-29) — Reliable Homeowner: prevents failure, preparedness mindset | 45 min, moderate
```

## Date Handling

**Calculate yesterday correctly:**
- Account for month boundaries: Jan 1 → Dec 31 (previous year)
- Account for varying month lengths: Mar 1 → Feb 28/29
- Use proper date calculation, not just "day - 1"

**Date format in annotations:**
- Use `MM-DD` format: `(from 12-29)`, `(from 01-15)`
- Zero-pad single digits: `01-05` not `1-5`
- **Preserve original dates**: Tasks rolled over multiple days keep their first rollover date
  - Example: Task from 01-15 → 01-16 → 01-17 stays as `(from 01-15)` throughout

## Common Scenarios

### No tasks in today's Todo
- Check yesterday for tasks to roll over
- If yesterday has tasks, present them for rollover
- If no tasks anywhere, inform user and exit

### No yesterday's daily note
- Skip rollover detection
- Just prioritize today's existing tasks
- Inform user that no yesterday note was found

### Manual rollovers already done
- Detect them via fuzzy matching
- Add annotations if missing
- Clean up yesterday's note
- Proceed with prioritization

### Tasks already have checkboxes
- Treat as unprioritized if they're in random order
- Re-prioritize and reorder based on ROI
- Preserve any existing completion status `[x]`

### Tasks with existing date annotations
- Tasks with `(from MM-DD)` annotations are multi-day rollovers
- **Preserve the original date** - do NOT update to yesterday's date
- Example: `(from 01-15)` stays as `(from 01-15)` even when rolled from 01-16 to 01-17
- This shows how long the task has been pending

## Red Flags

**Avoid these mistakes:**
- "I'll use a formula to calculate exact ROI scores" → NO, use reasoning and judgment
- "I'll skip rollover detection since tasks look different" → Use fuzzy matching, allow flexibility
- "I'll create a separate 'Today' section" → NO, work within `## Todo` section only
- "I'll leave tasks as bullets since they're already there" → Convert to checkboxes to show they're prioritized
- "I can't find a persona for this task" → Every task maps to one; look harder
- "I'll skip cleaning up yesterday's note" → Always remove rolled-over tasks from yesterday
- "I'll condense this long task description" → NO, preserve meaning exactly; only fix typos/formatting
- "I'll update the date annotation to yesterday" → NO, preserve original date for multi-day rollovers

## Rationalization Counters

| Excuse | Counter |
|--------|---------|
| "The tasks are too different to compare" | Every task has a persona + investment; compare those |
| "I need exact time/energy estimates" | Use reasonable judgment; doesn't need to be precise |
| "I can't name a persona for this task" | Look harder — every task reflects some identity |
| "Manual rollovers are too hard to detect" | Fuzzy matching handles minor differences; substantial similarity is enough |

## Example End-to-End

**Yesterday (12-29):**
```markdown
## Todo
- Fix leaky faucet
- [ ] Research vacation options (from 12-27) — Multi-day rollover, keeps original date
```

**Today (12-30) before skill:**
```markdown
## Todo
- fix leaky faucet
- Call insurance about bill
- Draft bike listing for marketplace
- Clean out garage corner
```

**User copied "fix leaky faucet" manually (note slight case difference)**

**Skill execution:**
1. Detects "fix leaky faucet" in both days (fuzzy match despite case difference)
2. Shows user: "Found: fix leaky faucet (in both), Available: Research vacation options (from 12-27)"
3. User selects both to roll over
4. Adds annotations, prioritizes all tasks
5. **Preserves (from 12-27) for vacation task** - doesn't change to (from 12-29)
6. Cleans up yesterday

**Today (12-30) after skill:**
```markdown
## Todo
- [ ] Call insurance about bill — Financially Responsible: protects household resources | 10 min, low
- [ ] Draft bike listing — Seller/Declutterer: income + lighter home compounds over time | 30 min, low
- [ ] Fix leaky faucet (from 12-29) — Reliable Homeowner: prevents failure, preparedness mindset | 45 min, moderate
- [ ] Clean garage corner — Organized Home: clarity + less mental load compounds daily | 20 min, low
- [ ] Research vacation options (from 12-27) — Intentional Family: shared experiences compound into memories | 30 min, low
```

**Yesterday (12-29) after cleanup:**
```markdown
## Todo
(empty or section removed if no tasks remain)
```

## Important Notes

- Always preserve YAML frontmatter and other sections in daily notes
- Work only within `## Todo` sections
- Use reasoning, not formulas
- Annotation format: `— Persona: compound yield | time, effort`
- Every task gets a persona — no exceptions
- Ask user if effort is genuinely unknown before ranking
- Fuzzy matching for flexibility in rollover detection
- Clean up yesterday's note after rollovers
