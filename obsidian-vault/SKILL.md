---
name: obsidian-vault
description: Load Obsidian vault context from any project. Reads vault conventions and enables read/write access.
---

# Obsidian Vault

Read `~/.claude/CLAUDE.md` for a `Vault: ~/path` line.

**Found:** Read `{vault}/CLAUDE.md` as authoritative context. You can now read/write vault files per those conventions.

**Not found:** Ask the user for their vault path, write `Vault: {path}` to `~/.claude/CLAUDE.md`, then read `{vault}/CLAUDE.md`. If the vault has no CLAUDE.md, note it and offer to create one.
