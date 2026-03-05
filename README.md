# Thinking Loops

OpenClaw skill. Sets up overnight AI thinking — 3 cycles that run while you sleep, each one trying to poke holes in the last. You wake up to a file with the reasoning and one thing to do today.

## What this actually looks like

8:04 AM, Telegram:

```
🌅 Today: Post your landing page in r/SaaS for feedback
⏱ ~45min
📎 Full reasoning attached
```

The attached file: 3 rounds of hypotheses, counter-arguments, competitor searches. Found 2 competitors I missed. Killed an idea I liked (market was taken). Landed on one action.

The more interesting part came after two weeks. The system had 14 days of data — which actions I did, which I skipped. It said:

> "9 out of 14 completed. All 9 were research or planning. All 5 skipped were outreach. You're not stuck on strategy. You're avoiding putting yourself out there."

I didn't see that pattern. It did.

## How it's different from ChatGPT

ChatGPT waits for you to open it. This runs at 2am whether you asked or not.

It runs 3 separate sessions overnight, each one reading the previous cold — like handing your draft to a different person. Claude's scheduled tasks need your laptop open. This runs on a server.

After a week it has your history. After two it sees patterns — what you do, what you dodge. ChatGPT starts blank every time.

Every cycle searches the web. Real competitors, real Reddit threads, real market data. Not 2024 training data.

## Install

```bash
clawhub install thinking-loops
```

Or clone from GitHub:
```bash
git clone https://github.com/davitttttt/thinking-loops ~/.openclaw/skills/thinking-loops
```

## Setup

Needs a machine that stays on overnight. VPS, home server, Raspberry Pi, whatever — as long as it doesn't sleep from 2am to 4am.

### Cron jobs

```bash
# Cycle 1 — reads your context, generates hypotheses, searches the web
openclaw cron add --name "think-1" --schedule "0 2 * * *" --target isolated --announce \
  --prompt "Thinking Loops: Cycle 1. Read all memory/ files and user context. What's really going on? Generate hypotheses. web_search required. Save to memory/night-cycle-\$(date +\%Y-\%m-\%d).md"

# Cycle 2 — tries to kill cycle 1's conclusions
openclaw cron add --name "think-2" --schedule "0 3 * * *" --target isolated \
  --prompt "Thinking Loops: Cycle 2. Read cycle 1 from today's night-cycle file. Challenge everything. Counter-arguments + web_search. Append to same file."

# Cycle 3 — picks one action, sends you the file
openclaw cron add --name "think-3" --schedule "0 4 * * *" --target isolated --announce \
  --prompt "Thinking Loops: Cycle 3 (Final). Read cycles 1-2. Compress into ONE action. Send the night-cycle file + 3-line morning message to the user's chat."
```

Change times for your timezone. Hour gaps between cycles matter — that's what makes cycle 2 actually challenge cycle 1 instead of just agreeing with itself.

### First run

The skill asks 4 questions when you first talk to it: what you're working on, what stage, what you're avoiding, how much time you have. Just answer normally.

### Then sleep

You'll get your first message tomorrow morning.

## How it works

```
02:00  Cycle 1 — hypotheses + web research
         ↓ (1 hour)
03:00  Cycle 2 — counter-arguments, kill weak ideas
         ↓ (1 hour)
04:00  Cycle 3 — one action → send to chat
```

Three separate AI sessions. Cycle 2 reads cycle 1's output like a stranger reviewing someone else's work. That gap is the whole point.

## Things that broke while we built this

**The AI caught our procrastination.** When we stalled on decisions, it started stalling too. "Maybe we should consider more options." We call this pattern contagion — the AI mirrors your dysfunction. Fixed it by making the system do the opposite: you hesitate, it decides. You scatter, it focuses.

**Thinking went in circles.** After 3 rounds with no new data, the output turned into reflection about reflection. Sounded deep, said nothing. Now every cycle has to search the web. If it can't find real data, it says so — but it can't skip the search.

**Five recommendations, zero done.** Early versions gave a list. Nobody did any of them. Now it picks one. If you skip it, it adjusts next time. But it never gives you a menu.

## Cost

Skill is free. API calls cost ~$4-8/month with Sonnet. Works with whatever model you have.

## License

MIT
