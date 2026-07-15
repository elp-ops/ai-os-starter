---
name: audit
description: Weekly gap check on what's actually working versus what's just a folder. Read-only, no changes made. Run on day 7, then weekly.
---

# Audit

Run this when the user runs `/audit` or asks something like "how set up am I actually."

## What this does

A read-only report on how much of this operating system is real versus placeholder. The goal is an honest score, not a flattering one.

## Steps

1. Check `context/` — is it still full of `{{placeholders}}`, or has it been filled in with real detail?
2. Check `.claude/rules/communication-style.md` — same check.
3. Check what's actually connected: open a fresh mental model and ask "if I were asked about this person's calendar, tasks, or inbox right now, could I actually answer, or would I have to guess?"
4. Check `decisions/log.md` — is it being used, or empty?
5. Check `memory/` — has anything actually been retained across sessions, or does every session start from zero?
6. Score each of the four areas (context, connections, capabilities, memory) as: not started / partial / working.
7. Report the score plainly, then recommend exactly one thing to fix next, the highest-leverage gap, not a full list.

## What this does not do

Doesn't fix anything itself. It's a diagnostic, not a repair.
