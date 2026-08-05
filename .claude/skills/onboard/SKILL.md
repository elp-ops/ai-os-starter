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
   - Want to name this operating system, and give your assistant a name and personality? We'd encourage it, it's the single biggest thing that makes this feel like yours instead of a generic tool. Totally your call to skip it, but if you're in: what do you want to call this operating system, what should your assistant be called, and what personality should they have?
3. Write the answers into:
   - `context/about-me.md`
   - `context/priorities.md`
   - `context/writing-samples.md`
   - `.claude/rules/communication-style.md`
   - All `{{Your Name}}` placeholders throughout `CLAUDE.md`, plus the `{{Filled by /onboard...}}` block under "Who {{Your Name}} is"
   - `# {{Ops Name — filled by /onboard. Default: {{Your Name}}'s Operating System}}` (line 1) and `{{Assistant Intro — filled by /onboard. Default: "You are {{Your Name}}'s assistant and thought partner inside this operating system. Your job: help them think clearly, decide faster, and get real work shipped, not just answer questions."}}` (line 3) in `CLAUDE.md`:
     - If they named things: line 1 becomes `# {{Ops Name}}` (their choice), line 3 becomes `You are {{assistant name}}, {{Your Name}}'s right hand inside {{Ops Name}}. {{personality description in their words}}\n\nYour job: help {{Your Name}} think clearly, decide faster, and get real work shipped, not just answer questions.`
     - If they skipped it: leave both lines unchanged (the defaults already embedded in the placeholders will remain).
4. Confirm what was filled in and suggest one thing to try asking right away, based on what they just described.

## What this does not do

Doesn't wire up any actual tool connections, that's a separate, incremental step, not part of this interview.
