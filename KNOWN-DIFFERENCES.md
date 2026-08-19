# Known Differences Between Editions

All three editions (Claude Code, Codex, Gemini CLI) run the same underlying
system. A few things work differently under the hood. Worth knowing, not
worth worrying about.

## Rules live inside the main file, not a separate folder

Claude Code automatically reads every file inside `.claude/rules/`, whatever
it's named. Codex and Gemini don't have that — they only automatically read
one file (`AGENTS.md` or `GEMINI.md`). So in those two editions, the same
rules (communication style, effort level, and so on) live as sections
inside that one file instead of separate files. Same content, different
location.

## How a skill gets triggered

- **Claude Code:** type `/skill-name`, or just describe what you need —
  Claude loads the matching skill automatically.
- **Codex:** describe what you need and it auto-loads the matching skill by
  matching your request against each skill's description, same as Claude
  Code. On top of that, you can manually pick one: run `/skills` to browse
  the list and choose, or mention a skill directly by name, e.g.
  `$skill-creator`. Both of these are documented by OpenAI itself, so
  they're confirmed to work.
- **Gemini CLI:** describe what you need and Gemini auto-loads the matching
  skill (via `activate_skill`) by matching your request against each
  skill's description, same idea as the other two. Gemini also has a
  `/skills list` command, but that only shows what's available — it's a
  management command, not a way to directly run one specific skill on
  demand. Gemini doesn't have a confirmed way to manually pick and run one
  skill the way Codex and Claude Code do. If you hit a case where
  auto-detection misses, say so and we'll fix the skill's description
  rather than assume manual invocation exists.

## Cost

- **Claude Code:** needs a paid Claude plan (Pro, Max, or API billing).
- **Codex:** needs a paid ChatGPT plan or API billing set up separately,
  check OpenAI's current plan page for which tiers include Codex access.
- **Gemini CLI:** has a free tier, but Google has been narrowing who can
  use it. Individual (non-enterprise) users are being shifted toward a
  newer tool called Antigravity CLI, the direct successor to Gemini CLI.
  If you're on an enterprise Gemini licence, this doesn't affect you.
  If you end up on Antigravity CLI instead, this kit already works there:
  `GEMINI.md` is read with zero changes, the only thing that needs moving
  is the skills folder, `.gemini/skills/` becomes `.agents/skills/`
  (Antigravity's first-run setup offers to do this for you automatically
  when it detects an existing Gemini CLI project).
