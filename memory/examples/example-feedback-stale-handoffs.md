---
name: example-feedback-stale-handoffs
description: EXAMPLE memory, illustrates what a real feedback entry looks like. Delete or replace with your own.
metadata:
  type: feedback
---

**This is a worked example, not a real memory.**

Rule: update a project's handoff file the moment something relevant changes during a session, not just once early on, and not only when explicitly asked. Treat it with the same live-update reflex as an active to-do list.

**Why:** an assistant wrote a handoff for a project right after finishing one piece of work. Later in the *same session*, it did more related work that changed the state described in that handoff, but only updated the to-do list, not the handoff itself. The user caught it: a stale handoff is worse than no handoff, because it makes the next session confidently act on outdated information instead of asking.

**How to apply:** any time work happens on something that already has a handoff file, check whether that work changes anything the handoff currently claims, and update it immediately if so. Don't wait for a session to end, or for someone to ask for an update, before the handoff reflects reality.

Notice the pattern: this and the other example in this folder both came from a real mistake, caught in the moment, corrected, and turned into a durable rule with its reasoning attached. That loop, not the folder structure, is what actually makes a system like this improve over time instead of repeating the same mistakes indefinitely.
