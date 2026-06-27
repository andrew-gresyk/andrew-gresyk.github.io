---
layout: post
title:  "Estimating a large game project with a spec-driven, LLM-assisted pipeline"
date:   2026-06-20 06:15:00 +0000
categories: features
---
# Estimating a large game project with a spec-driven, LLM-assisted pipeline

Estimating a multi-year game project is hard to do well, and harder to defend when someone asks how you reached the number. Most estimates come down to a single gut-feel figure. They tend to run optimistic, and the reasoning behind them is hard to reconstruct a month later. I wanted an estimate I could stand behind and update without starting over, so I built a workflow for it. Here is how it works and why it holds up.

![A layered, spec-driven estimation pipeline](/attachments/spec-driven-estimation.svg)

## Borrowing a habit from programming

Spec-driven development is normal practice now. You write the spec first, and it guides the design and the code. I applied the same idea to estimation, because the goals line up almost exactly.

A good spec makes your intent and assumptions explicit, catches problems early, and keeps a clear trail from the requirement to the result. That is also what a good estimate needs. Every number should have a source. Every assumption should be written down. The shaky ones should surface early, while they are cheap to fix, instead of during the pitch. An estimate is really just a spec for what something will cost, so treating it like one turned out to be a natural fit.

The paper trail is real, and that is the point. The estimate and the pitch come out to only about a tenth of the work. The other ninety percent is the facts, options, decisions, and plan they rest on. About half of that is verified primary-source facts, and roughly one document in seven is a verification or self-critique pass. So every number traces back to the work behind it.

## The pipeline

The work runs as six stages, and each one produces an artifact the next stage builds on:

1. **Primary-source facts.** Verified facts about the game and the tech, gathered from real sources and checked rather than assumed.
2. **Options.** For each system, the realistic ways to build it.
3. **Decisions.** The option we commit to, with the reason and the trade-off written down.
4. **Plan.** Workstreams, the order they happen in, the dependencies, and the critical path.
5. **Estimate.** The numbers, with ranges (more on that below).
6. **Pitch.** A summary a stakeholder can use to make a go or no-go call.

Because each stage gates the next, you cannot skip ahead. There are no options until the facts are checked, and no estimate until the plan holds together.

It also lines up with the old DIKW ladder. The facts are the data. The options turn that data into information. The decisions and the plan are the knowledge. The estimate on top is the judgment call. That is a handy way to see it: the pipeline climbs from raw facts to a decision you can act on, one checked step at a time.

## What makes the estimate defensible

The numbers are not single guesses. A few habits keep them honest:

- **Reference-class forecasting.** Every estimate is anchored to comparable shipped titles and reconciled against them, instead of starting from a blank page.
- **Three-point ranges.** Every line gets an optimistic, a likely, and a pessimistic number (PERT). Single-point estimates are not allowed, because they hide how uncertain a thing really is.
- **A risk register.** Each risk is scored by how likely it is and how much it would cost, with a named owner and a review at every milestone.
- **Sensitivity analysis.** How the budget and the schedule move if the team is smaller, the scope grows, or the timeline gets squeezed.
- **A per-discipline breakdown** that reconciles cleanly to a budget. The split is also a lever: handing the blueprint-friendly work to the content team cut one workstream's engineering cost by more than half.

The spread between the optimistic and pessimistic numbers is often the most useful output. A wide gap usually means the work is poorly understood, and that is worth talking about before anyone commits to it. None of this is exotic. It is standard practice in serious estimation, just applied to a game project, where a lot of estimating is still done by feel.

A quick note on the numbers below: they are rounded and anonymized to respect the client's NDA. The ratios are the point anyway, and they hold whatever the exact figures were.

> **From one estimate (a multi-year game):**
>
> - Engineering was about 30% of the total budget. The other 70% was content, art, audio, QA, and production.
> - The worst case ran about twice the best case. That gap is the whole reason for three-point ranges.
> - The first build was only about a quarter of the full lifecycle cost. Most of the work is integration, iteration, polish, QA, and fixes.
> - The risk buffer was about 10%, set from the register rather than guessed, and fewer than a fifth of the risks drove almost half of it.

## Where the LLM helps, and where it does not

The model does the broad work well: gathering source material, structuring it, cross-checking the decisions against the plan, and re-running the numbers in different breakdowns on demand. That breadth is most of the grind, and it is where the model saves real time.

What it does not get is the final say. Every figure is checked by hand against real shipped data, and the judgment stays with a person: the risk calls, the scope cuts, the sign-off. The model is a fast research and drafting partner. The value sits in the verification step between its output and the estimate you actually use.

## Three tiers, built for changing requirements

The tiers exist for one reason: client requirements change throughout a bid, and the estimate had to absorb that without a full redo each time. So rather than one fixed bill, it comes in three tiers:

- **Tier 1, the floor.** The complete product at baseline, true to the original, with none of the modern extras.
- **Tier 2, modernization and depth.** Optional upgrades for a current-gen feel and deeper systems. The client decides on Tier 2 at a set point in the schedule, not early and under pressure. On this project, choosing it added about 15% to the engineering effort.
- **Tier 3, the deferred shelf.** Nice-to-haves that are written down, priced, and parked for after launch. Out of the budget, but on the record.

Splitting it this way turns scope into a menu with prices. The client can see what the floor costs, what each upgrade adds, and what is safe to push to later.

When a request comes in, it usually means moving an item between tiers: pulling something from Tier 2 down into the floor, or lifting a Tier 1 item onto the shelf. Because the estimate is layered, re-pricing the new selection is quick. And the shelf is a pressure valve. Instead of turning a request down, you can say it fits well as a post-launch item, which keeps the conversation friendly and the budget intact.

## The part that paid off: changing scope

The biggest win showed up in client conversations.

Normally, when a client adds one feature and drops another, you end up redoing the whole estimate by hand. With this setup, the earlier facts stay put. You add the new ones, and the change flows downstream on its own. New facts feed new options, those feed the decisions and the plan, and you get a fresh estimate at the end. You only redo the parts that actually moved.

So when a client wanted to add, cut, or swap something, we did not start over. We updated the affected pieces and refreshed the pitch quickly, without reopening what had not changed. Reworking the scope went from a slow chore to something we could do while we were still in the room, and that changed the whole tone of the conversation.

## On scope, honestly

I owned the engineering estimate end to end. For the other disciplines, like art, design, audio, and production, the workflow produces first-order figures to round out the whole-project picture, and they are clearly flagged as rough. The rigor lives in the engineering core. The rest is a starting point for the people who own those areas, not a finished claim.

## What it adds up to

You end up with an estimate that has honest ranges and visible risks, one you can stand behind and update as fast as the conversation moves. For new business, that last part matters as much as the number itself.
