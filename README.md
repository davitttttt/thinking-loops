# Thinking Loops

**Autonomous overnight strategy for OpenClaw. Spots what you're avoiding. Delivers one action by morning.**

## Here's what I actually got this morning

My Telegram, 8:04 AM:

```
🌅 Today: Post your landing page in r/SaaS for feedback
⏱ ~45min
📎 Full reasoning attached
```

And the attached file had 3 cycles of thinking — hypotheses, counter-arguments, competitor research, everything stress-tested against real data. The AI found 2 competitors I didn't know about, killed an idea I was excited about (the market was already saturated), and landed on one thing worth doing today.

But the part that got me was week 2. After 14 days of daily actions, the system said:

> "You've completed 9 out of 14 actions. All 9 were research or planning. All 5 you skipped involved reaching out to people. You're not stuck on strategy — you're avoiding external validation."

It was right. I wouldn't have seen that pattern myself.

## Why not just use ChatGPT?

Three real differences, not marketing:

1. **It thinks while you sleep.** Three separate AI sessions run overnight, each one challenging the previous. By morning, weak ideas are dead and one action survives. ChatGPT waits for you to open it. Claude's scheduled tasks only work while your laptop is open.

2. **It has your full history.** After a week, it knows what you do and what you avoid. After two weeks, it sees patterns you don't. ChatGPT starts fresh every conversation.

3. **It pulls real data every cycle.** Competitors, Reddit threads, market signals — not training data from 2024, but what people are saying right now. Every cycle searches the web. No exceptions.

## Install

```bash
clawhub install thinking-loops
```

## Setup

You need a machine that stays on overnight — a VPS, home server, Raspberry Pi, or a laptop that doesn't sleep. The skill runs cron jobs from 2am to 4:30am.

### Step 1: Add cron jobs

```bash
# Cycle 1 — Analyze everything, generate hypotheses
openclaw cron add --name "think-1" --schedule "0 2 * * *" --target isolated --announce \
  --prompt "Thinking Loops: Cycle 1. Read all memory/ files and user context. What's really going on? Generate hypotheses. web_search required. Save to memory/night-cycle-\$(date +\%Y-\%m-\%d).md"

# Cycle 2 — Try to kill cycle 1's conclusions
openclaw cron add --name "think-2" --schedule "0 3 * * *" --target isolated \
  --prompt "Thinking Loops: Cycle 2. Read cycle 1 from today's night-cycle file. Challenge everything. Counter-arguments + web_search. Append to same file."

# Cycle 3 — Compress to one action, deliver to chat
openclaw cron add --name "think-3" --schedule "0 4 * * *" --target isolated --announce \
  --prompt "Thinking Loops: Cycle 3 (Final). Read cycles 1-2. Compress into ONE action. Send the night-cycle file + 3-line morning message to the user's chat."
```

Adjust times for your timezone. The key is 1-hour gaps between cycles.

### Step 2: Talk to it

On first interaction, the skill asks 4 questions to understand your situation. Just answer naturally — no forms.

### Step 3: Sleep

Tomorrow morning you'll get your first message.

## How it works

```
02:00  Cycle 1 — Hypotheses + web research
         ↓ (1 hour gap — fresh session)
03:00  Cycle 2 — Counter-arguments, kill weak ideas
         ↓ (1 hour gap — fresh session)
04:00  Cycle 3 — Compress → 1 action → deliver to chat
```

Each cycle is a completely separate AI session. Cycle 2 reads cycle 1's file cold, like a different person reviewing your work. That's why the time gaps matter — it's not one long response pretending to self-correct.

## What we learned building this

We've been running this on ourselves for weeks. Three things kept breaking:

**Pattern Contagion.** The AI started mirroring our bad habits. When we procrastinated, it got cautious — "maybe we should explore more options" instead of "do this." We built in the opposite: when you hesitate, it decides. When you scatter, it focuses. Complement, not mirror.

**Thinking expires.** After 3 cycles without real data, the AI produces reflection about reflection — sounds smart, means nothing. Now every cycle is required to search the web. No data, no cycle.

**Too many options kills action.** Early versions gave 5 recommendations. Nobody did any of them. Now it gives one. Just one. If you don't do it, it adapts — but it never gives you a menu.

## Cost

The skill is free. You pay for API calls:
- **~$4-8/month** with Sonnet (3 cycles/night)
- Works with any model OpenClaw supports

## License

MIT
