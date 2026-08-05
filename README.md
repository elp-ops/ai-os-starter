# ELP-Ops Starter Kit

A free, MIT-licensed starter kit that turns Claude Code into your personal operating system: one place that holds your context, your rules, your recurring skills, and your memory across sessions.

Clone it, run `/onboard`, and it personalises itself to you.

**New to git, VS Code, or Claude Code entirely?** Start with [GETTING-STARTED.md](GETTING-STARTED.md) instead, it walks through everything from zero.

## Quick start

1. Clone this repo to a working folder on your machine.
2. Open it in Claude Code (via VS Code or the desktop app).
3. Run `/onboard`. Answer honestly, paste real writing samples rather than describing your voice. Takes about 15 minutes.
4. Use it for a week. Bring it real questions and real decisions.
5. Run `/audit` after your first week to see what's actually wired up versus what's just a folder.
6. Run `/level-up` weekly to find and ship one automation at a time.

## Repo layout

```
ai-os-starter/
├── README.md
├── CLAUDE.md                  ← Your operating manual (filled in by /onboard)
├── LICENSE
├── .gitignore
├── .claude/
│   ├── rules/                  ← Always-on behaviour:
│   │   ├── communication-style.md
│   │   ├── effort-level.md      ← Right-size effort/model to the task
│   │   ├── cost-efficiency.md   ← Cheapest model that gets the job done
│   │   ├── build-philosophy.md  ← Systems over hacks, structure before code
│   │   ├── notifications.md     ← Signal when a task's actually done
│   │   └── data-home.md         ← Where documents/notes live (Notion by default, your call)
│   └── skills/
│       ├── onboard/            ← One-time setup interview
│       ├── audit/              ← Recurring gap check: what's actually connected vs. just a folder
│       ├── level-up/           ← Weekly: find one thing worth automating, ship it
│       ├── grill-me/           ← Think something through out loud, one question at a time, checkpointed to disk
│       ├── session-handoff/    ← Clean stopping point before you clear a long conversation
│       ├── skill-builder/      ← Build and quality-check your own skills once you outgrow the starter set
│       └── security-audit/     ← Run before any app or website you build goes live, no exceptions
├── context/
│   ├── about-me.md, priorities.md, writing-samples.md   ← Filled by /onboard
│   └── handoffs/                ← One file per project, read automatically when you resume it
├── decisions/
│   └── log.md                   ← Append-only record of decisions and why
├── memory/                      ← Persistent, cross-session memory (see memory/README.md)
│   └── examples/                 ← Two real, genericised feedback memories, worked examples
├── brainstorms/                 ← Raw /grill-me capture files, one per topic
├── projects/                    ← Your actual project folders live here
├── templates/                   ← Reusable templates as you build them
├── references/                  ← Reference patterns, e.g. when to bring in multiple models
└── archives/                    ← Old stuff. Don't delete. Move here.
```

## Why this structure

Four things have to be true for an AI assistant to actually be useful day to day, not just in a demo:

1. **It knows you** — your business, your priorities, your voice (`context/`, `CLAUDE.md`).
2. **It can reach your stuff** — calendar, docs, whatever tools you actually use, wired up incrementally, not all at once.
3. **It can do things**, not just answer questions, turning a repeatable task into a reusable skill (`.claude/skills/`).
4. **It remembers**, across sessions, not just within one conversation (`memory/`).

Build in that order. Skipping ahead (wiring up ten tools before the assistant even knows who you are) is why most of these setups stall.

## Not a theory, tested against months of real use

This isn't a folder structure someone sketched out once. It's the result of several months of building and running an AI operating system day to day: an engineering and finance background, full marketing training, and hands-on AI work, applied directly to real work in Claude Code, not read about secondhand.

That kind of real use surfaces real discrepancies: places where the assistant's behaviour drifted, made an assumption it shouldn't have, or handled something the wrong way. Every one of those is a small failure worth catching, not ignoring. The operating practices below, and the memory system in particular, exist specifically to catch that kind of thing once and have it stay fixed, instead of the same mistake happening again next week. See `memory/examples/` for two real (genericised) cases of exactly that.

## The operating practices

Beyond the folder structure, a handful of behavioural rules are what actually make this feel like an operating system rather than a chatbot with extra files:

- **Session handoffs** so you never re-explain context (`/session-handoff`, writes to `context/handoffs/`)
- **A relentless discovery interview** for stress-testing a plan or extracting what's in your head before you start building (`/grill-me`, writes to `brainstorms/`)
- **Drive the session**, the assistant leads with what's done/open/next instead of waiting to be asked (`CLAUDE.md`)
- **Effort and cost calibration**, right-sized to the task, not maxed out by default (`.claude/rules/effort-level.md`, `cost-efficiency.md`)
- **A real memory system** with distinct types (user, feedback, project, reference) and an always-loaded index, not just a folder of notes (`memory/README.md`)
- **A verification habit** at the end of multi-step work: what was required, what's missing, what was assumed
- **A done signal**, so background work doesn't leave you wondering (`.claude/rules/notifications.md`)

## License + attribution

MIT License. The folder pattern, the onboard/audit/level-up skill rhythm, and the operating practices above were built and refined through months of actual daily use. The content, rules, and specific frameworks in this repo are original.
