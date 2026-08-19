---
name: skill-builder
description: Use when creating new skills, optimizing existing skills, auditing skill quality, running evals or benchmarks on a skill, or tuning skill trigger descriptions for better accuracy.
---

## What This Skill Does

Guides the creation and optimization of Gemini skills using confirmed best practices. Use this whenever:

- Building a new skill from scratch
- Optimizing or auditing an existing skill
- Deciding how a skill should be structured and invoked
- Troubleshooting a skill that isn't working correctly

For the complete technical reference on frontmatter fields, file structure, and troubleshooting, see [reference.md](reference.md).

## Quick Start: What Is a Skill?

A skill is a reusable set of instructions that tells Gemini how to handle a specific task. Skills live in `.gemini/skills/[skill-name]/SKILL.md` inside your project. Gemini scans skill descriptions at session start and calls its `activate_skill` tool automatically when your request matches one (you'll see a confirmation prompt before it actually loads). Run `/skills list` to see what's available, `/skills reload` to refresh after adding a new one.

There is no confirmed way to invoke one specific skill directly by name on demand. The only path in is describing your task in language that matches the skill's `description`.

Think of skills as SOPs for Gemini. Instead of re-explaining a workflow every conversation, you write it once and let matching requests invoke it automatically.

**How they work under the hood:**
- Your project's `GEMINI.md` instructions are always loaded, every conversation
- Skill *descriptions* (from frontmatter) are always loaded so Gemini knows what's available
- The full skill content only loads when the skill is actually activated
- Once loaded, Gemini follows the skill's instructions while still respecting your GEMINI.md rules

---

## Mode 1: Build a New Skill

When building a new skill, run the **Discovery Interview** first. Do NOT start writing files until discovery is complete.

### Discovery Interview

Ask questions one round at a time. Each round covers one topic. Move to the next round only after the user answers. Keep going until you're 95% confident you understand the skill well enough to build it without further clarification.

**Round 1: Goal & Name**
*Why this matters: A clear goal prevents scope creep. The name is how the skill is described in `/skills list`, so it needs to be memorable and specific.*

- What does this skill do? What problem does it solve or what workflow does it automate?
- What should we call it? (Suggest a name based on their answer -- lowercase, hyphens, max 64 chars)

**Round 2: Trigger**
*Why this matters: The `description` field is the only thing Gemini matches against to decide whether to activate your skill. Bad trigger words mean Gemini never uses it. Too broad means it fires when you don't want it.*

- What would someone say to trigger this? (Get 2-3 natural language phrases)
- Does it accept input? If so, what? (e.g., a topic, a URL, a file path)

**Round 3: Step-by-Step Process**
*Why this matters: Gemini follows instructions literally. Vague steps produce vague results. Specific steps produce consistent output every time.*

- Walk me through exactly what should happen from trigger to output. What's step 1? Step 2? Keep going.
- For each step: does Gemini do it directly, or does part of it get delegated (e.g. to a script)?
- Does this need to be conversational (back-and-forth with the user) or is it a fire-and-forget task?

**Round 4: Inputs, Outputs & Dependencies**
*Why this matters: Skills that don't specify where to find inputs or where to put outputs produce inconsistent results. Nailing this down makes the skill reliable.*

- What inputs does the skill need? (Files, API responses, user-provided details, live data)
- What does it produce? (Files, text output, structured data) Where do outputs go?
- Does it need external APIs, scripts, or tools? Which ones?
- Does it need reference files, style guides, templates, or examples?

**Round 5: Guardrails & Edge Cases**
*Why this matters: Skills without guardrails can produce unexpected behavior -- wrong outputs, unnecessary API costs, or actions you didn't intend.*

- What could go wrong? What are the common failure modes?
- What should this skill NOT do? Any hard boundaries?
- Are there cost concerns? (API calls, AI image generation, etc.)
- Any ordering or dependency constraints? (e.g., "must check X before doing Y")

**Round 6: Confirmation**
*Why this matters: Misunderstandings caught here save you from rebuilding the skill later.*

After all rounds, summarize your understanding back to the user in this format:

```
## Skill Summary: [name]

**Goal:** [one sentence]
**Trigger:** [natural language phrases that should activate it]
**Input:** [what it accepts, or "none"]

**Process:**
1. [step]
2. [step]
...

**Inputs:** [what it reads/needs]
**Outputs:** [what it produces + where]
**Dependencies:** [APIs, scripts, reference files]
**Guardrails:** [what can go wrong, what to avoid]
```

Ask: "Does this capture it? Anything to add or change?" Only proceed to building once the user confirms.

**Skipping rounds:** If the user provides enough context upfront (e.g., they describe the full workflow in their first message), skip rounds that are already answered. Don't re-ask what you already know.

### Build Phase

Once discovery is complete, build the skill following these steps:

**Step 1: Choose the skill type**

- **Task skills** (most common) give step-by-step instructions for a specific action. Activated when the request matches the description. Examples: generate a report, summarize a PR, deploy code.
- **Reference skills** add knowledge Gemini applies to current work without performing an action. Examples: coding conventions, API patterns, style guides.

**Step 2: Configure frontmatter**

Set these fields based on what you learned in discovery:

- `name` -- Matches the directory name. Lowercase, hyphens, max 64 chars.
- `description` -- Written as: "Use when someone asks to [action], [action], or [action]." This is the only signal Gemini uses to decide whether to activate the skill, so make it specific and keyword-rich.

Only set fields you actually need. Don't add frontmatter just because you can.

For the full field reference, see [reference.md](reference.md).

**Step 3: Write the skill content**

Structure task skills as:
1. **Context** -- Files to read, APIs to call, reference material to load
2. **Step-by-step workflow** -- Numbered steps. Each step tells Gemini exactly what to do.
3. **Output format** -- What the result looks like. Include templates, file paths, structured formats.
4. **Notes** -- Edge cases, constraints, what to delegate, what NOT to do.

Content rules:
- Keep SKILL.md under 500 lines. Move detailed reference material to supporting files.
- If the skill takes input, describe in plain language what it expects and how it's used in the steps below.
- Be specific about any delegation to a script or external tool -- include the exact command to run.
- Specify all file paths (inputs, outputs, scripts, references).

**Step 4: Add supporting files (if needed)**

If your skill needs detailed reference docs, examples, or scripts, add them alongside SKILL.md in the same directory. Reference them from SKILL.md so Gemini knows they exist. Supporting files are NOT loaded automatically -- they load only when Gemini needs them. See [reference.md](reference.md) for the full pattern.

**Step 5: Document in GEMINI.md**

Your project's `GEMINI.md` file is where Gemini loads project-wide instructions every conversation. After creating a skill, add a one-line entry to its skills list so you (and your team) know what's available:

- Skill name
- Trigger phrases
- Brief description of what it does
- Output location (if it produces files)

This isn't required for the skill to work, but it keeps your project organized and helps Gemini understand how skills fit into your broader workflow.

**Step 6: Test**

There is no confirmed way to run one specific skill directly on demand, so testing means confirming the skill activates the way you intend:

1. **Natural language** -- Say something matching the description. Does Gemini call `activate_skill` for it?
   - If not, revise the `description` field to include the keywords you used
   - Try 2-3 different phrasings to verify it triggers reliably
2. **Discovery check** -- Run `/skills list` to confirm the skill is discovered and shows the description you expect. Run `/skills reload` if you just added or edited it.
3. **Edge cases** -- Try phrasing the request vaguely, or with missing details, and see whether it still activates sensibly.

If issues arise, see Troubleshooting in [reference.md](reference.md).

**Step 7: Iterate with the feedback cycle**

The first run will not be the best run. Use this cycle every time you improve a skill:

1. **Watch it run in full** -- Do not walk away the first few times. Watch every step. This is how you spot waste.
2. **Identify redundant work** -- Is the skill making the same API call every run to find the same IDs, paths, or config? Hardcode those directly in the skill file. Static information should not be re-fetched dynamically.
3. **Keep verbose work out of context** -- If the skill involves heavy searching, verbose API responses, or large output, write that output to a file and have the skill reference the file, rather than dumping it all into the conversation.
4. **Prefer markdown over live calls** -- If a skill needs reference data (API docs, style guides, project context), store it as a local markdown file and point the skill to it. Reading a local file is faster and cheaper than an API call or web search.
5. **Add rules for repeated mistakes** -- If the same error keeps appearing, add a guardrail to the skill. Do not rely on fixing it in chat each time.
6. **Update the skill after every meaningful change** -- The skill is the source of truth. If you corrected something in chat, reflect it in the file immediately.

Expect 5 to 10 cycles before a skill is genuinely reliable. That is normal.

### Complete Example

Here's a minimal but complete skill you can use as a starting template:

**File:** `.gemini/skills/meeting-notes/SKILL.md`

```yaml
---
name: meeting-notes
description: Use when someone asks to summarize meeting notes, recap a meeting, or format meeting minutes.
---

## What This Skill Does

Takes raw meeting notes and produces a structured summary with action items.

## Steps

1. Ask the user to paste their raw meeting notes (or provide a file path).
2. Extract the following from the notes:
   - **Attendees** -- Who was in the meeting
   - **Key decisions** -- What was decided
   - **Action items** -- Who owes what, with deadlines if mentioned
   - **Open questions** -- Anything unresolved
3. Format the output using the template below.
4. If a meeting title was given, use it. Otherwise, infer a title from the content.

## Output Template

# Meeting: [title]
**Date:** [date if mentioned, otherwise "Not specified"]
**Attendees:** [comma-separated list]

## Key Decisions
- [decision]

## Action Items
- [ ] [person]: [task] (due: [date or "TBD"])

## Open Questions
- [question]

## Notes

- Keep summaries concise. Don't add commentary or embellish.
- If notes are too vague to extract action items, flag that to the user instead of making them up.
```

---

## Mode 2: Audit an Existing Skill

Use this checklist to audit any existing skill. Read the skill file first before running through the checklist. Fix issues before marking the audit complete.

### Frontmatter Audit

- [ ] `name` matches the directory name
- [ ] `description` uses natural keywords someone would actually say when they need this skill
- [ ] `description` is specific enough to avoid false triggers but broad enough to catch real requests
- [ ] `description` states clearly when the skill should, and should not, trigger
- [ ] No unnecessary fields are set (don't add frontmatter just because you can)

### Content Audit

- [ ] Total SKILL.md is under 500 lines (detailed reference moved to supporting files)
- [ ] Clear step-by-step workflow with numbered steps (for task skills)
- [ ] Output format is specified with templates or examples
- [ ] All file paths and locations are documented
- [ ] Any delegation to a script or external tool includes the exact command to run
- [ ] Notes section covers edge cases, constraints, and what NOT to do
- [ ] No vague instructions -- every step tells Gemini exactly what to do
- [ ] Instructions are specific about what input the skill expects and how it's used

### Integration Audit

- [ ] Skill is documented in GEMINI.md (recommended, not required)
- [ ] Supporting files (if any) are referenced from SKILL.md, not orphaned
- [ ] Scripts (if any) have correct file paths and are executable
- [ ] API keys (if any) are stored in environment variables, never hardcoded

### Quality Audit

- [ ] A beginner could follow the instructions without prior context
- [ ] Instructions are actionable, not abstract
- [ ] Keeps verbose output (long searches, big API responses) out of the main conversation where possible
- [ ] Doesn't duplicate information that lives elsewhere (GEMINI.md, other skills)
- [ ] Output paths follow a predictable convention

### Optimization Opportunities

After running the audit, check [reference.md](reference.md) for more on structuring supporting files and other confirmed patterns.

---

---

## Mode 3: Eval a Skill

Use this when you want to test whether a skill is producing good outputs and identify where it's falling short.

### What you need

Either:
- A set of test cases you provide in chat (input + expected output pairs), or
- A `tests/` folder inside the skill directory containing test case files

Test case format (plain text or markdown):
```
## Test: [descriptive name]
Input: [what you would say to trigger the skill, or the detail passed]
Expected: [what a good output looks like -- can be a description, not literal text]
```

### Eval process

1. Read the skill file fully before starting.
2. For each test case:
   - Simulate running the skill with the given input
   - Compare the actual output (or expected behaviour) against the expected result
   - Score: Pass / Partial / Fail
   - Note what specifically went wrong for Partial and Fail cases
3. Produce an eval report in this format:

```
## Eval Report: [skill-name]
Date: [today]

| Test | Result | Notes |
|------|--------|-------|
| [name] | Pass/Partial/Fail | [what went wrong, if anything] |

Pass rate: X/Y (Z%)

### Identified issues
- [issue 1]
- [issue 2]

### Suggested skill improvements
- [specific change to make in the skill file]
```

4. Ask: "Want me to apply these improvements to the skill now?"
5. If yes, apply changes and re-run the failing tests to confirm they now pass.

**Rules:**
- Do not invent test cases. Only evaluate against what was provided.
- A "Pass" means the skill would produce output that matches the intent of the expected result. It does not have to be word-for-word identical.
- Partial means the output is directionally correct but missing key elements.
- Always end with specific, actionable improvement suggestions -- not vague feedback.

---

## Mode 4: Benchmark a Skill

Use this to compare performance with the skill loaded vs without it, so you can measure how much uplift the skill actually provides.

### Process

1. Ask for 3 to 5 benchmark prompts. These should be representative requests that would normally trigger the skill. If the user doesn't have any ready, generate them based on the skill's description and purpose.

2. For each prompt, run it twice:
   - **With skill**: Load the skill and execute the prompt following the skill's instructions
   - **Without skill**: Execute the same prompt using only Gemini's default behaviour, no skill instructions

3. For each pair, assess:
   - **Quality**: Which output better meets the intent of the prompt? (With / Without / Tie)
   - **Completeness**: Which covers more of what was asked?
   - **Consistency**: Does the with-skill output reliably match expected format and structure?

4. Produce a benchmark report:

```
## Benchmark Report: [skill-name]
Date: [today]
Prompts tested: [N]

| Prompt | Winner | Reason |
|--------|--------|--------|
| [short label] | With skill / Without / Tie | [one sentence] |

Overall uplift: [positive / neutral / negative]
Recommendation: [Keep as-is / Improve skill / Consider retiring]

### Notes
- [any patterns observed]
```

5. If the skill shows negative or neutral uplift, flag it clearly and recommend whether to improve or retire it.

**Rules:**
- Be honest about ties. If the outputs are equivalent, say so.
- "Without skill" does not mean Gemini is bad -- it means the skill may not be adding value.
- If the skill is an encoded preference (specific workflow, not capability uplift), weight consistency more heavily than quality.

---

## Mode 5: Trigger Tuning

Use this when a skill is not triggering when expected, or triggers in situations where it shouldn't.

### Process

1. Read the skill's current `description` field from frontmatter.
2. Generate 15 to 20 natural language phrases a user might say when they want this skill. Vary the phrasing: formal, casual, short, long, explicit, vague.
3. For each phrase, assess: would the current description cause Gemini to activate this skill? (Yes / No / Uncertain)
4. Identify the gaps:
   - **False negatives**: phrases that should trigger the skill but probably won't
   - **False positives**: phrases that would trigger it incorrectly (if relevant)
5. Rewrite the description to capture the true positives and reduce false triggers.

Output format:
```
## Trigger Tuning: [skill-name]

### Test phrases
| Phrase | Would trigger? | Should trigger? |
|--------|---------------|-----------------|
| [phrase] | Yes/No/Uncertain | Yes/No |

### Current description
[paste current description]

### Issues found
- [false negative: phrase X wouldn't trigger but should]
- [false positive: phrase Y triggers but shouldn't]

### Revised description
[new description]

### Changes made
- [what changed and why]
```

6. Apply the revised description to the skill file. Ask before applying.

**Rules:**
- The description should use natural language keywords people actually use, not abstract capability language.
- Keep descriptions concise. Longer is not better -- clarity and keyword density matter more.
- If the skill has many siblings (lots of other skills in the project), ensure the revised description doesn't overlap with adjacent skills. Check other skill descriptions before finalising.

---

## Recommended Conventions

Adapt these to fit your project:

- Skills live in `.gemini/skills/[skill-name]/SKILL.md`
- Output files go in a predictable location (e.g., `output/[skill-name]/`)
- API keys go in environment variables, never hardcoded in skill files
- Document all active skills in your project's GEMINI.md
- Frontmatter `description` is written as: "Use when someone asks to [action], [action], or [action]."

## Important Notes

- Always read an existing skill before optimizing it. Never propose changes to a skill you haven't read.
- When building a new skill, check if a similar skill already exists that could be extended instead.
- For file structure patterns, see [reference.md](reference.md).
- Skills are self-annealing by default: when a skill errors mid-run, fix the immediate problem AND patch the skill file so the same error cannot recur. See [reference.md](reference.md) for the full self-annealing spec.
