# Notifications

Optional, but recommended: after every multi-step task, send yourself a signal that it's done, rather than having to keep checking back. This is the "cadence" piece, the assistant tells you when it's actually finished, instead of you polling.

This is platform-specific, set it up for whatever you actually use:

- **macOS:** `osascript -e 'display notification "Done" with title "Your Assistant"'`
- **Other platforms:** a Slack/Telegram message to yourself, a webhook, whatever reaches you reliably.

Only fire this for genuinely multi-step work, not every single reply, otherwise it becomes noise you tune out.
