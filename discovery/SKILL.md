---
name: discovery
description: Recall-biased vault file discovery. Run early and liberally — before reading anything. When in doubt, run this first.
---

# Discovery

Given context (a task, question, or topic), find vault files likely to be relevant.

**Signals to extract:**
- Keywords and topics
- Named entities: people, projects, notes
- Temporal language ("recently", "last week") → also check git log or file mtimes

**Steps:**
1. Extract signals from context
2. Search file names and content for matches
3. If temporal signals present, supplement with git log / mtime
4. Return a lightly ranked candidate list — recall-biased, more is fine

**Output:** File paths with one-line rationale each. Flag confidence (strong / possible).

Present results and let the user or calling agent narrow down.
