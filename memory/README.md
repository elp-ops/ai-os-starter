# Memory

What the assistant remembers across sessions, not just within one conversation. This is the most load-bearing folder in the whole system, get this working well and everything else compounds.

Don't hand-edit these files, let the assistant manage them as it learns things worth retaining. If something's wrong or outdated, say so directly and have it correct the file, that way the reasoning behind the correction gets captured too, not just the fix.

## Four types of memory

**User** — who you are, your role, your preferences, your knowledge. Shapes how responses get tailored to you specifically, not written generically.

**Feedback** — corrections *and* confirmations. Every time you correct an approach ("don't do that") or confirm a non-obvious choice worked ("yes, exactly"), that's worth saving. Structure: the rule itself, then *why* (the reasoning, often a past incident), then *how to apply it* (when this kicks in). The why is what lets the assistant judge edge cases later instead of blindly following a rule out of context.

**Project** — ongoing work, decisions, deadlines, who's doing what and why. Decays fast, always convert relative dates ("Thursday") to absolute ones when saving, so it's still readable months later.

**Reference** — pointers to where things live in external systems (which Notion database, which Slack channel, which dashboard). Not the content itself, just where to find it.

## The index

`MEMORY.md` is the always-loaded index, one line per memory file, linking out to the full content. Keep it short, it's context budget spent on every single session whether it's needed or not. The actual memory files only get read when relevant.

## What NOT to save here

Anything derivable from the code or file structure itself, git history, debugging fix recipes (the fix is in the code, the commit message has the context), or ephemeral task state that only matters for the current conversation. If it'll be true and useful in three months, it's memory. If it's just what you're doing right now, it's not.

## Before trusting an old memory

A memory that names a specific file, function, or flag is a claim that it existed *when the memory was written*, not proof it exists now. Verify before acting on it, especially before recommending something from memory as if it's current fact.

## Worked examples

`memory/examples/` has two real (genericised) feedback memories, not placeholders, actual discrepancies caught during months of daily use of this system, corrected, and turned into durable rules. Read them to see what a real feedback memory looks like: the failure, the reasoning, and when the rule applies, not just an instruction. Delete the `examples/` folder once you understand the pattern and have your own real memories accumulating.

