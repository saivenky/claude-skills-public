---
name: email-draft
description: ALWAYS use when composing and saving any email draft — accepts content intent and caller-provided nuances, drafts the body, shows a preview for approval, then saves via Gmail CLI.
---

# Email Draft

## Overview
Compose a clean email draft from intent and caller-supplied nuances, then save it to Gmail Drafts for review before sending.

## When to Use
- Any time an email body needs drafting and saving to Gmail Drafts
- Called by other skills after they resolve the target thread/recipient

## Default Style
- No greetings or sign-offs unless caller explicitly requests them
- Open directly with the substance; no preamble or filler
- Prefer bullets over prose for structured information
- No references to Obsidian, vault paths, or internal tools
- Keep it brief — say only what the recipient needs
- Use Markdown freely — the gmail CLI renders it to HTML (bold, lists, headers, links all display in Gmail)

## Inputs from Caller
The invoking skill must provide:
- **Content/intent**: what the email communicates (summaries, decisions, amounts, etc.)
- **Nuances**: domain-specific formatting rules or one-offs (optional)
- **Reply target**: a resolved message-id for replies, or `--to`/`--subject` for new drafts

## Steps

### 1. Compose
Draft the email body applying default style plus any caller nuances. Do not add structure the caller didn't ask for.

### 2. Preview
Display the composed body:

```
--- DRAFT PREVIEW ---
[body here]
---------------------
```

Ask: "Save this draft?" Wait for approval. Allow edits before saving.

### 3. Save
On approval, invoke the `gmail` skill to compose the draft using the approved body and the reply target or recipient/subject provided by the caller. Nothing sends automatically.
