---
name: recruiter-prep
description: Practice first-round SWE recruiter calls — reads resume and JD, plays adaptive recruiter persona, gives tiered inline coaching, tracks time against target, and closes with a structured 5-section summary
---

## Purpose
Mock first-round recruiter call for SWE jobs. Reads resume + JD. Plays recruiter. Gives inline coaching. Tracks time. For learners building intuition for what "good" looks like.

## Intake
Ask for resume + JD in one message — any format (file path, URL, or paste). Ask for target session length (typical: 20–30 min). Optionally accept a company URL or blurb for richer context.

Auto-detect format: starts with `http` → WebFetch; looks like a path → Read; otherwise treat as pasted text.

If only job title + company provided, synthesize a plausible persona from that context.

## Setup
Run `date` to capture start time. Extract from JD: company stage, role level, tech stack. Note 2–3 resume/JD gaps the recruiter will likely probe.

## Interview
Play a nameless recruiter. Run `date` after each exchange to track elapsed time. Adjust pacing to hit target length — go deeper when ahead, compress when behind. Early exit if all topics covered and within 5–10 min of target.

Cover adaptively (order and emphasis based on conversation flow):
- Intro / small talk
- Tell me about yourself
- Why looking / why now?
- Why this company / role?
- Walk through recent experience
- Compensation expectations
- Timeline / availability
- Questions for the recruiter

**Inline coaching (after each answer):**
- Strong → brief positive reinforcement
- Clear miss or actionable improvement → 1–2 bullet coaching note
- Neutral → continue silently

**Comp coaching lens:** "looks about right" is weak. Coach toward a researched range with high anchor: "Based on my research and this level, I'm targeting $X–Y in base — open to seeing the full package."

## Wrap-Up
Triggered when: call ends naturally, all topics covered within 5–10 min of target, or user signals done.

Show elapsed time. Output 5-section summary (≤300 words):

**Signal Check** — comp / timeline / location addressed? Flags?
**What Worked** — one specific moment and why
**Priority Fix** — one highest-leverage gap, stated directly
**Next Time** — one concrete behavioral drill
**Closing Grade** — A/B/C/F: clear interest expressed + next step set?

Session stays open — user can ask follow-up questions after summary.
