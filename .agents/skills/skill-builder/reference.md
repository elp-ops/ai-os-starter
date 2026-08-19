# Skill Builder Reference

Technical reference for Codex skills: what a `SKILL.md` file needs, where skills live, and how to structure supporting files. Scoped to what's actually confirmed in Codex's own documentation — a few advanced patterns that exist in other coding assistants aren't documented for Codex, so they're left out here rather than guessed at.

Source: https://learn.chatgpt.com/codex/build-skills

---

## Understanding AGENTS.md vs Skills

This is the most important concept to understand before building skills. Where you put instructions determines how Codex uses them.

| | AGENTS.md | Skill |
|---|---|---|
| **When loaded** | Every conversation, always | Only when invoked (by name or auto-detection) |
| **What it's for** | Project-wide rules, conventions, context | Specific task workflows, specialized procedures |
| **Size concern** | Always in context, so keep it focused | Only loaded when needed, but keep under 500 lines |
| **Examples** | "Use TypeScript for all files", "Run tests before committing", API conventions | "Generate a PR summary", "Create meeting notes", "Deploy to staging" |

**Rule of thumb:** If Codex should *always* know it, put it in AGENTS.md. If Codex should only know it when doing a specific task, make it a skill.

AGENTS.md instructions still apply inside a skill's execution. A skill inherits your project's rules -- it doesn't override them. Think of it like layers: AGENTS.md is the base layer, and the skill adds task-specific instructions on top.

---

## Frontmatter Field Reference

Codex's own documentation confirms two `SKILL.md` frontmatter fields:

| Field | Description |
|-------|-------------|
| `name` | The skill's identifier. Lowercase letters, numbers, hyphens. Matches the directory name. |
| `description` | Explains when the skill should, and should not, trigger. Codex uses this to decide when to load the skill automatically. |

Only `description` is really required in practice -- without a clear one, Codex has nothing to match your requests against.

---

## Skill File Locations

Where you store a skill determines who can use it:

| Location | Path | Applies to |
|----------|------|------------|
| Personal | `~/.agents/skills/<name>/SKILL.md` | All your projects |
| Project | `.agents/skills/<name>/SKILL.md` | This project only |

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

Reference them from SKILL.md so Codex knows they exist. Supporting files are NOT loaded automatically -- they load only when Codex needs them.

### Visual Output (Bundled Scripts)

Skills can bundle scripts that generate visual output (HTML, images, charts). This extends what Codex can produce beyond text.

```yaml
---
name: codebase-visualizer
description: Generate an interactive tree visualization of your codebase
---

# Codebase Visualizer

Run the visualization script:

```bash
python .agents/skills/codebase-visualizer/scripts/visualize.py .
```
```

The skill provides orchestration instructions while the bundled script does the heavy lifting. This pattern works for dependency graphs, test coverage reports, API docs, or any visual output.

---

## Troubleshooting

### Skill not triggering

If Codex doesn't use your skill when expected:

1. **Check the description** -- Does it include keywords users would naturally say? The description is how Codex decides to load the skill.
2. **Verify it's visible** -- Ask "What skills are available?" to confirm Codex sees it.
3. **Rephrase your request** -- Try wording that more closely matches the description.
4. **Invoke directly** -- Run `/skills` and select it to confirm the skill works at all.

### Skill triggers too often

If Codex uses your skill when you don't want it:

1. **Make the description more specific** -- Narrow the trigger conditions.

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

- **Skills guide:** https://learn.chatgpt.com/codex/build-skills
- **AGENTS.md:** https://developers.openai.com/codex/guides/agents-md
