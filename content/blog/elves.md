+++
title = "The Shoemaker's Elves: my agents draft the work before I ask"
date = "2026-10-03"
description = "A small local harness that drafts my work before I ask. My only job is review. How the loop works and how its feedback becomes rules I can read."
tags = ["agents", "tools"]
images = ["/elves/poster.png"]
draft = false
+++

Most of us still use AI the old way. Something comes up, you kick off an agent, and then you babysit it. Every task starts with you.

There is a new way: a harness that runs on its own, watches for what is being asked of you, and drafts the work before you ever ask. That is the loop Muse, Grok Bot, and dots all launched around last month, each on someone else's computer and on a paid plan.

I have been running a small, free, local version of that loop on my laptop. It is not a competitor to any of them. It is the same loop at a size one person can run, read, and change. I call it elves.

You know the story. The shoemaker goes to bed. In the morning the shoes are finished, and all he does is look them over.

Imagine coming back from lunch, a meeting, or a couple of hours of deep work, and every important ask that came in while you were away has already been drafted and is waiting for your review. No triaging messages. No kicking off agents and babysitting them.

{{< video src="/elves/elves-explainer.mp4" poster="/elves/poster.png" captions="/elves/elves-explainer.vtt" >}}

*Two-minute explainer. The repo is [github.com/myaa2913/elves](https://github.com/myaa2913/elves).*

## The loop

There are four commands, and a scheduler runs the first one.

- **`/scan`** runs hourly. It reads only the sources named in one `config.yaml`, decides what needs action, and triages each thread: draft, quick reply, delegate, clarify, or ignore.
- **`/draft`** runs the moment a scan finds work. Each task gets a subagent that reads a small wiki first (my style, the requester's page, the project's page, the playbook for that kind of output) and writes its draft to a folder. It does not ask permission to start.
- **`/review`** is the only part I do. Every item opens with the same line: *Action taken: read X, drafted Y, nothing sent.* I approve, edit, ask for a redraft, or discard.
- **`/ingest`** turns my reactions into rules in the wiki, so the next draft is better.

State lives in a sqlite file, a folder of drafts, and a folder of markdown. There is nothing else.

## Feedback becomes rules

The part I care most about is the last step. The hosted products say their agent "learns your preferences." I wanted to be able to read what was learned.

Say I react to a draft with *"Too long, Sam just wants the number."* Ingest does not store that sentence. It rewrites one line on `wiki/people/sam.md`:

> Prefers the headline number first; skip methodology unless asked. `[raw/feedback/2026-09-25.md]`

Three rules keep the wiki honest. Feedback is distilled into a rule, never pasted in as a quote. Every rule cites the raw transcript it came from, and raw transcripts are never edited. A newer rule rewrites the older one instead of sitting beside it, so a page reads as what is true now. The idea is Andrej Karpathy's LLM wiki, applied to one person's work: the wiki is compiled knowledge and the agent is the compiler.

Because it is plain markdown with wiki links, I can open the folder in Obsidian, see everything it believes about how I work, and fix what is wrong by hand.

## Running it unattended

The loop only pays off if the scan runs without me at the keyboard. Two things made that work. The scheduled run gets an explicit list of tools it may use, and every write tool a connector exposes (send, reply, forward, delete) is denied in the harness, so "nothing ships" does not depend on the model following an instruction. And drafting never asks permission to start, because a harness that asks "shall I draft this?" has put me back in the scheduler's seat.

It deliberately does not drive a browser, sign into websites, or send anything, and it has no cloud component. It runs while my laptop is awake, which is also when I am working.

## Try it

The repo is not my implementation, which is tied to my connectors and full of my mail. It is one idea file, about 200 lines of markdown: the setup interview, the database schema, each command, and notes on running it unattended. Hand it to whatever coding agent you already use and say *"Read this and build it. Start with `/setup`."* Then change whatever you like.

[github.com/myaa2913/elves](https://github.com/myaa2913/elves), MIT licensed.
