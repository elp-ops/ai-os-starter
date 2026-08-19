# Getting Started (Codex Edition)

This is the real starting point if you're new to all of this. If you're already comfortable with git and the command line, the shorter "Quick start" in `README.md` will get you there faster. This one assumes you've never done any of it before.

## The problem

Since computers arrived, work hasn't gotten less administrative. It's gotten more. Logging things. Re-typing the same information into five different tools. Re-explaining yourself, from scratch, to a system that doesn't remember you.

That's admin. It's not judgement, and judgement is the one part of your work that's actually yours, the part nobody else can do for you.

**The shift underway is simple: less time on admin, more time on the calls only you can make.** An assistant that remembers you and carries context forward instead of making you re-supply it every time is what makes that shift real.

There's leverage in this too. Every correction you give it, every decision it captures, becomes an asset you keep building on, not something that resets. And because it's yours, it's not locked to one company's app: swap what's running underneath it in five years, and the system, the memory, the judgement you've built into it, moves with you.

## What you're about to get

Right now, your life runs across a pile of separate apps: notes in one place, calendar in another, docs somewhere else, decisions nowhere at all. None of them talk to each other. None of them remember you.

Humans aren't built to hold all of that in their heads, re-explaining themselves to every tool, every time. We're wired to want things connected.

This is the start of something else: teaching one assistant, your right hand, to be that connective layer across your world. Not another app doing one job. Something that learns how you work and starts pulling the pieces together for you.

Every week you use it, it's building a real picture of you: your decisions, your patterns, your priorities. That's your data, compounding, working for you, not sold off to train someone else's product. The earlier you start teaching it, the more it's worth to you later.

You're not just installing a tool. You're at the start of building something that gets more valuable to you the longer you run it.

### Why you can't just ask any AI to do this

You can type "be my assistant" into any AI chat right now. It'll say yes. Then you close the tab, and every bit of that goes with it.

The real work is the plumbing underneath: where does it write down what it learns about you? How does it pick up a project you started two weeks ago without you re-explaining it? What stops it from forgetting the one thing you told it never to do without asking first?

That plumbing takes months to get right, not because it's clever, but because it only gets proven by running it every day and fixing what breaks. This kit is that plumbing, already built, already tested on real use, not theory.

### What actually changes

**Without this kit:** a blank Codex window. Every session starts from zero. No memory of what you told it yesterday. No way to pick up a project without re-explaining it. No structured way to even think through a plan before building it, let alone catch it drifting off course.

**With this kit:** a memory that survives between conversations. It picks up exactly where you left off. A place for your priorities, your voice, your decisions. A way to stress-test a plan before you build it. You stay in the loop, correcting it as it goes.

### It's not a second brain. It's a right hand.

You'll hear this kind of thing called a "second brain." We don't use that term here. You already have one brain. Think of this more like training a right hand: an assistant you teach, that gets sharper and more useful the more you work with it.

### Why it still needs you

Think of it like teaching a kid to ride a bike. You don't hand over the bike and walk off, hoping they work it out on their own. You're there for every wobble, correcting as they go. Left alone too early, a kid on a bike can build the wrong habits, or just pedal off somewhere you didn't want them to go.

Same idea here. This only gets better if you're in the loop: correcting it when it gets something wrong, telling it what worked and what didn't. Leave it running with no feedback and it can drift, start making its own calls, go rogue instead of staying aimed at what you actually want. You stay the one steering, always.

By the end of this guide, you'll have a working, personalised assistant of your own. Takes about 30-45 minutes.

## Before you start: it costs money

Codex needs a ChatGPT Plus, Pro, or Team plan, or API billing set up separately. Sort this out before you start installing anything, so you're not surprised by a paywall halfway through.

## Name your assistant

Before you install a single thing, think about this: what do you want to call this operating system, and what do you want to name your assistant?

We'd genuinely encourage you to do this, not just skip past it. A name and a personality is the actual difference between "an AI tool" and an assistant you're building a real working relationship with. It's the single biggest thing that makes this feel like *yours*.

You don't have to decide anything right now, and you can absolutely skip it if it's not your thing. You'll be asked for real in a few minutes, during setup. Just start turning it over in your head.

## Setup

### Step 1: Create your account and confirm your plan

Go to [chatgpt.com](https://chatgpt.com) and sign up. Then make sure you actually have an eligible plan active: ChatGPT Plus, Pro, or Team, or API billing set up separately if you'd rather go that route. Codex won't run without one.

### Step 2: Install VS Code

**On a Mac:**
1. Go to [code.visualstudio.com](https://code.visualstudio.com).
2. Click the big "Download for Mac" button.
3. Open the file that downloads, drag Visual Studio Code into your Applications folder.
4. Open Visual Studio Code from Applications.

**On Windows:**
1. Go to [code.visualstudio.com](https://code.visualstudio.com).
2. Click "Download for Windows."
3. Run the installer that downloads. Click through with the default options.
4. Open Visual Studio Code when it finishes.

### Step 3: Install the Codex extension

1. Inside VS Code, click the Extensions icon in the left sidebar (it looks like four small squares).
2. Type "Codex" into the search box.
3. Click Install on "Codex – OpenAI's coding agent," the extension published by OpenAI.
4. A new Codex icon will appear in the sidebar. If it doesn't show up right away, open the Command Palette (Cmd+Shift+P on a Mac, Ctrl+Shift+P on Windows) and run "Codex: Open Codex Sidebar." Click it and sign in with the ChatGPT account you made in Step 1.

### Step 4: Get this repository onto your computer

(A repository, or "repo," is just a folder of files that git keeps track of, so you can save changes and go back to earlier versions if you need to.)

1. Inside VS Code, click the Source Control icon in the left sidebar (it looks like a branching line).
2. Click "Clone Repository."
3. Paste this address exactly: `https://github.com/elp-ops/ai-os-starter`
4. If VS Code tells you it needs to install git first, click "Install," wait for it to finish, then click "Clone Repository" again.
5. Choose a folder on your computer to save it in (Documents works fine), then click "Select as Repository Destination."
6. When it finishes, click "Open" to open the folder in VS Code.

### Step 5: Run `/onboard`

1. Click the Codex icon in the sidebar to open the Codex sidebar.
2. Type `/onboard` and press enter.
3. Answer honestly, one question at a time. When it asks for writing samples, paste something you actually wrote — a real email, text, or caption — don't describe how you write, show it.
4. It'll ask if you want to name your operating system and give your assistant a name and personality. Say yes if you want your copy to feel like it's actually yours.
5. This whole interview takes about 15-20 minutes.

## Where should your data live?

At some point you'll want somewhere to keep notes, documents, and research, not just chat messages that scroll away. You've got two real options:

**Connect a free Notion plan.** Good if you want a visual, organised home for your notes and documents that you can browse and search on its own, outside of Codex entirely. If you pick this, just ask your assistant to walk you through connecting Notion, it's a few minutes and it'll handle the setup with you.

**Run everything directly through Codex, as local files in this repo.** Simpler. No new account to set up. Works fine if nobody else needs to see or use the data.

Here's the actual principle behind the choice: pick based on **who else needs to read the data**, not just whichever tool sounds more official. As an example: one project keeps its records in Google Docs and Sheets instead of Notion, purely because the people who need to see that data don't have Notion and already know Google. Match the tool to the audience.

Whichever you choose, keep the data in an actual, visible file somewhere, not just buried inside a chat. That matters for two reasons: you can actually look at it and review it later, and it's safer, nothing important only exists as a message that could get lost.

Once you've decided, tell your assistant, and ask it to update the Where Your Data Lives section in `AGENTS.md` so it stops guessing.

## What's inside

A quick tour of what you just cloned:

- **`AGENTS.md`** — the instructions your assistant reads every single time it starts talking with you. This is where its name, personality, and how it should behave all live, including its always-on rules (communication style, effort and cost calibration, build philosophy, and more), folded directly into this one file rather than split across separate rule files.
- **`context/`** — notes about you: who you are, what you're working on, how you like to communicate.
- **`context/handoffs/`** — a "pick up where we left off" note for each project, so you never have to re-explain something you already explained last time.
- **`decisions/`** — a running list of decisions you've made and why, so you can look back later and remember the reasoning.
- **`memory/`** — what your assistant remembers between conversations: things you've told it, corrections you've made, patterns it's noticed over time. This is managed automatically, you shouldn't need to edit it by hand.
- **`projects/`** — your actual projects live here, one folder each.
- **`templates/`** — anything reusable you build, like a checklist or an email format, so you're not rebuilding it from scratch every time.
- **`references/`** — background material worth keeping around.
- **`archives/`** — old stuff that's not active anymore, kept instead of deleted.
- **`.agents/skills/`** — specific things you can ask your assistant to *do*, not just ask about, like `/onboard`, `/audit`, and `/level-up`.

## If something goes wrong

**VS Code says it can't clone because git isn't installed.** Click "Install" when it offers to, wait for it to finish, then try "Clone Repository" again. On a Mac, this sometimes opens a separate Apple installer window, let that finish too before retrying.

**The Codex icon doesn't show up after installing the extension.** Quit VS Code completely and reopen it. If it still doesn't show up, open the Command Palette (Cmd+Shift+P on a Mac, Ctrl+Shift+P on Windows) and run "Codex: Open Codex Sidebar."

**You typed `/onboard` and nothing happened, or it says the command isn't found.** Make sure you opened the actual `ai-os-starter` *folder* in VS Code (File → Open Folder), not just a single file. The Codex sidebar needs to be pointed at the right folder to see its commands.

**You're signed in but Codex won't respond, or keeps asking you to sign in again.** Double check your plan is actually active: log into chatgpt.com and check Settings → Billing (or your API billing dashboard, if you went that route).

**You got a red error message while cloning.** Double check you copied the address exactly: `https://github.com/elp-ops/ai-os-starter`

**VS Code says you don't have permission to push.** Expected. Your copy is still linked to the original public repo, which you don't own. You don't need to push for any of this to work, everything lives on your computer. If you want your own backup copy later, ask your assistant to set you up a private GitHub repo, and make sure it's private, your context files have personal information in them.

## This isn't a finished product

Once you're set up, it's yours to shape. As you actually use it, test what fits how you work and change what doesn't. Nothing here is fixed.

**The people who get the most out of this don't set it up once and leave it.** They keep building on it: a new skill here, an adjusted rule there, one small improvement at a time. That's what the built-in `/audit` and `/level-up` skills are for, they exist to keep you doing exactly that.

## What to do next

- Use it for a real week. Bring it actual questions and actual decisions, not just tests.
- After the first week, run `/audit`. It'll tell you honestly what's actually being used versus what's just an empty folder.
- Every week after that, run `/level-up`. It finds one repeatable task worth turning into something automatic, and helps you ship it.
