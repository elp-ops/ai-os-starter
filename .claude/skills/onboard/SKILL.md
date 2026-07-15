---
name: onboard
description: One-time setup interview that fills in context/, CLAUDE.md, and .claude/rules/communication-style.md. Run on day one, or any time the user's situation has changed and the context needs refreshing.
---

# Onboard

Run this when the user says something like "I just cloned this, help me get set up" or explicitly runs `/onboard`.

## What this does

Interviews the user, then writes the answers into the right files. Fully personalises the operating system from a generic template into something specific to them.

## Steps

1. Check whether `context/about-me.md` and `context/priorities.md` already have real answers, not `{{placeholders}}`. If yes, this is a refresh, confirm what's changing rather than starting over.
2. Ask these questions, one at a time, not all at once:
   - Who are you, what do you do, who do you serve?
   - Paste one or two things you've actually written recently, verbatim, not described. (This is for voice matching, not summarising.)
   - What are your two to three biggest priorities for the next quarter?
   - What are the 5 to 7 places your most important data actually lives? (Calendar, task tool, CRM, comms, docs, whatever's real for you.)
   - How do you want this assistant to communicate with you: format, tone, things to avoid?
   - Is there anything this assistant should never do without asking first?
3. Write the answers into:
   - `context/about-me.md`
   - `context/priorities.md`
   - `context/writing-samples.md`
   - `.claude/rules/communication-style.md`
   - The `{{Your Name}}` and knowledge-base placeholders in `CLAUDE.md`
4. Confirm what was filled in and suggest one thing to try asking right away, based on what they just described.

## What this does not do

Doesn't wire up any actual tool connections, that's a separate, incremental step, not part of this interview.
