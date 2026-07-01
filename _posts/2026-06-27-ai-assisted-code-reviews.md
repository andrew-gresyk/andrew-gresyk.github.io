---
layout: post
title:  "AI-Assisted Code Reviews"
date:   2026-06-27 08:20:00 +0000
categories: features
---
# How I review every change with a panel of AI agents

For a side project, I review every change with four different AI coding agents instead of one, and then I check their work by hand. It sounds like overkill, and for a one-line fix it would be. But for anything with real risk in it, a small panel of agents plus a verification habit catches a lot that a single agent would wave through. This is the longer version of how it works, why it works, and where it doesn't.

The project is a complex real-time UE5/C++ simulation I build on my own. None of what follows depends on the project itself. It's about the review process, which is the part worth sharing.

![AI-assisted code review pipeline](/attachments/ai-assisted-code-reviews.svg)

## The loop the review sits in

I don't start with code. Each piece of work moves through a fixed set of stages, and each stage produces a written artifact the next one builds on: an idea, then research, then a spec, then a design, then a plan, then the implementation, then a short retro on how it went.

Most of that is ordinary. Writing a spec before code, gating the plan before you build, keeping the artifacts in version control: that's all standard practice in 2026, and plenty of tools ship it out of the box. I'm not claiming any of it as new.

Two points in the loop are review gates. The first is before any code exists: the spec, design, and plan get reviewed together, as one set, to catch drift between them. The second is after the implementation: the diff gets reviewed against those approved artifacts. The interesting part is what "reviewed" means for me.

## What the panel actually is

At each gate I don't ask one agent to look. I run the same review through four separate products, Claude Code, Codex, Cursor, and opencode, and on top of that a fan-out across a few more free models. Each one reads the change on its own and writes its findings to its own file, with every finding marked as something to fix, to defer, or to accept as is.

The reason for four products rather than one model called four times is that they behave differently. Each gathers its own context, prompts in its own way, and has its own blind spots, so they don't all flag the same things. The variety is the point. The specific tools matter less; if one of them falls behind next year, I'll swap it out.

## Does it actually work? Here are the numbers

I went back through the project's git history for the stretch where the process was consistent, items 08 through 15, and counted. Across those, the panel wrote about 461 findings in 170 review files. Of those findings, 81 percent were fixed, 3 percent were deferred, and 16 percent were accepted with a note and no change. Even the cheap free models had high fix rates.

That's a real result, and I want to be careful about what it does and doesn't show.

What it shows: the findings the agents wrote up were, overwhelmingly, worth acting on. This wasn't a pile of noise I waded through. Most of it was real.

What it doesn't show: that the panel beats one strong reviewer. I have no single-reviewer control to compare against, and I don't dedupe findings across the agents, so the same issue caught by three of them counts three times. I can't prove the extra agents each added something a lone good reviewer would have missed. There's also a selection effect: git only holds the findings I thought were worth writing down, not the raw output I threw away, so these numbers can't tell you how noisy the agents are before that filter. And it's one developer on one project. Treat it as an experience report, not a benchmark.

## One finding, start to finish

A concrete example, kept generic. On one item, a single reviewer flagged that a comment still described the old default behaviour after a later change had already flipped it. None of the other reviewers mentioned it. I opened the file, confirmed the comment was stale, and fixed it. Small, but it's the kind of thing that rots a codebase quietly, and it surfaced only because that one agent happened to read that file closely.

That's the shape of most of what the panel catches. Not dramatic bugs, but drift: stale comments, a plan that has wandered from its own spec, a test that doesn't check what it claims to. Boring, and worth catching.

## Why verifying matters more than counting heads

Here's the part that surprised me. More reviewers is not automatically better. There's research showing that naïve multi-agent groups can do worse than their best single member, because they average strong and weak opinions together. Collect votes from a crowd of agents and go with the majority, and you can land below where one good reviewer would have put you.

So the panel isn't the clever bit. The verification is. When several agents land on the same issue on their own, I treat it as close to certain, because independent agreement is hard to fake. A lone flag is a lead to check, not a verdict. And I read every finding against the actual code before I act on it, because looking is the only way to know which ones hold up. The agents widen the net; the checking is what turns a catch into something I can trust.

## What's actually new here, and what isn't

I'll be straight, because this is easy to oversell. Spec-driven development is standard. A shared instruction file that lets one process run across different agents is standard. Even "a team of AI agents playing different software roles" is an off-the-shelf idea now.

The part I haven't seen others do is run the same review across several rival agent products at once and treat their disagreement as signal. The closest things I can find are ensembles that call several models behind one interface, which isn't the same as four separate products with their own tooling. That's the one piece I'd call genuinely uncommon, as of mid-2026, and even then I'd rather show the method than argue about who got there first.

## It isn't free

Running four products plus a fan-out costs time, tokens, and the upkeep of keeping their instructions in sync. So I don't do it for everything. I match the review to the change. A one-line fix gets a glance; a tricky change or a new system gets the full panel. The discipline is matching effort to risk, not running everything through the whole machine.

## If you want to try it

You don't need four agents to get most of the benefit. Start with two you already use, run them on the same change, and add one habit: don't act on a finding until you've checked it against the code yourself. Treat each agent as a useful but fallible reader, not an oracle. The value isn't in any single read. It's in having a couple of independent ones and a person who verifies before anything changes.

That's the whole thing. It's a calm, slightly boring process, and that's mostly why it works.
