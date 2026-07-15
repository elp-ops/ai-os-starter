---
name: level-up
description: Weekly ritual to find one repeatable manual task and turn it into a shipped skill or automation. Run weekly, ideally after /audit.
---

# Level Up

Run this when the user runs `/level-up` or asks "what should I automate next."

## What this does

Finds exactly one thing worth automating this week, scopes it, and gets it built. Not a brainstorm of ten ideas, one shipped thing.

## Steps

1. Ask what's felt repetitive or tedious this week. If nothing comes to mind, check `decisions/log.md` and recent conversation for anything that came up more than twice.
2. For the strongest candidate, ask: is this worth automating fully, partially, or not at all right now? Not everything should be automated, some things are cheaper to just do manually.
3. If it's worth building: scope the smallest version that's actually useful, not the most complete version imaginable.
4. Build it as a skill (if it's a repeatable prompt pattern) or flag it as a bigger build if it needs real code/integration.
5. Log the decision in `decisions/log.md`: what was chosen, why, what was explicitly left out of scope.

## What this does not do

Doesn't force a build every week if there's genuinely nothing worth automating. Skipping a week honestly beats manufacturing busywork.
