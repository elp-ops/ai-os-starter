# Skill Builder Reference

Technical reference for Gemini skills: what a `SKILL.md` file needs, where skills live, and how to structure supporting files. Scoped to what's actually confirmed in Gemini CLI's own documentation -- a few advanced patterns that exist in other coding assistants aren't documented for Gemini, so they're left out here rather than guessed at.

Source: https://geminicli.com/docs/cli/skills/

---

## Understanding GEMINI.md vs Skills

This is the most important concept to understand before building skills. Where you put instructions determines how Gemini uses them.

| | GEMINI.md | Skill |
|---|---|---|
| **When loaded** | Every conversation, always | Only when activated (matched by description) |
| **What it's for** | Project-wide rules, conventions, context | Specific task workflows, specialized procedures |
| **Size concern** | Always in context, so keep it focused | Only loaded when needed, but keep under 500 lines |
| **Examples** | "Use TypeScript for all files", "Run tests before committing", API conventions | "Generate a PR summary", "Create meeting notes", "Deploy to staging" |

**Rule of thumb:** If Gemini should *always* know it, put it in GEMINI.md. If Gemini should only know it when doing a specific task, make it a skill.

GEMINI.md instructions still apply inside a skill's execution. A skill inherits your project's rules -- it doesn't override them. Think of it like layers: GEMINI.md is the base layer, and the skill adds task-specific instructions on top.

---

## Frontmatter Field Reference

Gemini CLI's own documentation confirms two `SKILL.md` frontmatter fields:

| Field | Description |
|-------|-------------|
| `name` | The skill's identifier. Lowercase letters, numbers, hyphens. Matches the directory name. |
| `description` | What the skill does and when it should be used. This is the only signal Gemini matches your request against to decide whether to activate the skill automatically. |

Only `description` is really required in practice -- without a clear one, Gemini has nothing to match your requests against.

---

## How Activation Works

Gemini scans skill descriptions at the start of a session. When your request matches a skill's `description`, Gemini calls its `activate_skill` tool -- you'll see a confirmation prompt before the skill's full content actually loads into the conversation. Once activated, the `SKILL.md` body and its folder structure are added to the conversation so Gemini can follow the instructions and reach any supporting files.

There is no confirmed command or syntax for a user to invoke one specific skill directly on demand. The management commands below let you see and control what's discoverable, but the only way to actually run a skill is to phrase a request that matches its description closely enough for `activate_skill` to fire.

### `/skills` Management Commands

| Command | What it does |
|---------|---------------|
| `/skills list [all] [nodesc]` | Lists discovered skills. `all` includes disabled ones, `nodesc` hides descriptions for a shorter view. |
| `/skills reload` (or `/skills refresh`) | Re-scans skill directories for new or edited skills. |
| `/skills link <path> [--scope user\|workspace]` | Links a skill from a local directory into discovery. |
| `/skills disable <name>` | Turns a discovered skill off. |
| `/skills enable <name>` | Turns a previously disabled skill back on. |

---

## Skill File Locations

Where you store a skill determines who can use it and, when two skills share a name, which one wins. Gemini resolves skills across four discovery tiers, lowest precedence first:

| Tier | Path | Applies to |
|------|------|------------|
| Built-in | Bundled with Gemini CLI | Everyone, lowest precedence |
| Extension-bundled | Provided by an installed extension | Wherever that extension is enabled |
| User | `~/.gemini/skills/<name>/SKILL.md` (or `~/.agents/skills/<name>/SKILL.md`) | All your projects |
| Workspace | `.gemini/skills/<name>/SKILL.md` (or `.agents/skills/<name>/SKILL.md`) | This project only, highest precedence |

Within each tier, the `.agents/skills/` alias takes precedence over the `.gemini/skills/` path if both exist.

---

## Advanced Patterns

### Supporting Files

Skills can include multiple files in their directory. Keep SKILL.md under 500 lines and move detailed reference material to separate files.

```
my-skill/
  SKILL.md              # Main instructions (required, <500 lines)
  reference.md          # Detailed docs
  scripts/
    helper.py           # Utility script
```

Reference them from SKILL.md so Gemini knows they exist. Supporting files are NOT loaded automatically -- they load only when Gemini needs them.

### Visual Output (Bundled Scripts)

Skills can bundle scripts that generate visual output (HTML, images, charts). This extends what Gemini can produce beyond text.

```yaml
---
name: codebase-visualizer
description: Generate an interactive tree visualization of your codebase
---

# Codebase Visualizer

Run the visualization script:

\`\`\`bash
python .gemini/skills/codebase-visualizer/scripts/visualize.py .
\`\`\`
```

The skill provides orchestration instructions while the bundled script does the heavy lifting. This pattern works for dependency graphs, test coverage reports, API docs, or any visual output.

---

## Troubleshooting

### Skill not triggering

If Gemini doesn't activate your skill when expected:

1. **Check the description** -- Does it include keywords users would naturally say? The description is the only thing `activate_skill` matches against.
2. **Verify it's discovered** -- Run `/skills list` to confirm Gemini sees it and isn't disabled.
3. **Reload after edits** -- Run `/skills reload` if you just created or changed the skill; discovery doesn't always pick up changes mid-session.
4. **Rephrase your request** -- Try wording that more closely matches the description.
5. **Check it isn't disabled** -- Run `/skills enable <name>` if a previous `/skills disable` turned it off.

### Skill triggers too often

If Gemini activates your skill when you don't want it:

1. **Make the description more specific** -- Narrow the trigger conditions.
2. **Disable it when not needed** -- Run `/skills disable <name>` to turn it off without deleting the file.

---

## Self-Annealing (Default Behavior)

Skills self-heal. When a skill runs and produces an error, the agent must immediately patch `SKILL.md` (and any supporting files or scripts) so the same failure cannot recur. This is not optional — it is the default way skills work.

### Process

1. Run the skill normally.
2. An error occurs: script crash, API error, wrong output format, missing dependency, auth failure, etc.
3. Fix the immediate problem so the current run succeeds.
4. Patch the skill file with the fix. Goal: if the skill runs again tomorrow with zero context, the same error is impossible.
5. Resume the original task.

### What to patch

- Wrong values (API endpoints, model IDs, auth headers, config paths) — correct them in the skill.
- Missing steps (missing dep install, missing auth check, missing env var) — add the step to the skill instructions.
- Flawed logic in scripts (bad parsing, wrong API call sequence, missing error handling) — fix the script and add a comment explaining why.
- Incorrect assumptions (file paths, data formats, service behavior) — update the skill to document the actual behavior.

### What NOT to patch

- One-off environmental issues (network timeout, rate limit, disk full). Not skill bugs.
- User input errors. Don't anticipate every possible bad input.

The principle: every error is a lesson the skill learns permanently. Skills converge toward zero-failure execution over time.

---

## Related Documentation

- **Skills guide:** https://geminicli.com/docs/cli/skills/
- **GEMINI.md (memory/context):** https://geminicli.com/docs/cli/gemini-md/
