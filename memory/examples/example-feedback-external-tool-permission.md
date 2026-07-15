---
name: example-feedback-external-tool-permission
description: EXAMPLE memory, illustrates what a real feedback entry looks like. Delete or replace with your own.
metadata:
  type: feedback
---

**This is a worked example, not a real memory. It's here to show what "catching a discrepancy and fixing it durably" actually looks like in practice.**

Rule: never write to an external tool (a shared doc, a database, a live page someone else edits) without an explicit, real-time go-ahead. Answering a planning question ("which approach do you prefer") is not the same as authorising execution.

**Why:** this came from a real incident. An assistant took a user's answer to a multiple-choice planning question as permission to immediately execute a multi-step edit on a shared document. The user hadn't actually authorised that, they were still just deciding on an approach. The assistant got stopped mid-edit, and because it hadn't fetched and saved the original content before overwriting it, some of the damage couldn't be reversed from its own side, the user had to recover it manually through the tool's own version history.

**How to apply:** before any write to a system outside this repo (a shared doc, a live database, anything someone else might be actively using), get a separate, explicit "yes, do it now," not an inference from an earlier planning answer. Before any destructive edit, fetch and hold the current content first, so a revert is actually possible without depending on the external tool's own history feature.

This is exactly the kind of thing worth capturing the moment it happens: the specific failure, the reasoning behind the fix, and when the rule applies. A rule without the *why* is just an instruction to follow blindly. A rule with the why lets the assistant judge edge cases correctly later.
