---
layout: default
title: "High End and Workhorse Models Appear: GPT 5.6 Sol and Grok 4.5"
permalink: /high-end-workhorse-models/
author: "Lee Harrington"
date: 2026-07-09
---

# High End and Workhorse Models Appear: GPT 5.6 Sol and Grok 4.5

OpenAI released their Mythos/Fable competitor **GPT 5.6 Sol**. Word is that it's not quite *as* good at the most difficult tasks as Anthropic's Fable 5, but for almost all tasks it's just as good and **70% cheaper** to run—and faster.

xAI released **Grok 4.5**, now in the hunt for the top. It's not *quite* there, but it's allegedly very good and a *lot* cheaper: cheaper per token, and more efficient at getting work done using fewer tokens.

## The labor pyramid of AI models

We are well into the "labor pyramid" of AI models. You have your expensive human solutions architect, supported by a cheaper project manager, then a set of mid-level workers and some cheap newbies. Nobody staffs a project with just the highest-priced, most capable humans. Likewise with AI these days.

## A real workflow: Sol plans, Terra supervises, Grok builds

I wanted to make a significant refactor of my personal website. I used GPT 5.6 Sol to take my long list of changes and evaluate the site's use as a public portfolio and as my own personal operating OS / dashboard for apps and the like.

I told Codex on my Linux box to set up the ability to run Cursor with Grok 4.5 Medium. It's wonderful how you can just direct these things that way—I didn't have to know how to set that up myself.

I gave Sol's output to Codex using **GPT-5 Terra** (the not-as-expensive model you use for most day-to-day tasks). I had Terra act as the supervisor, handing work to Cursor / Grok 4.5 Medium. I set up a loop:

1. Here's the mission.
2. Use Grok for the work.
3. Check his work.
4. If it doesn't meet mission requirements, direct Grok to fix it.
5. Keep going until everything is complete.

And it did.

## How'd Grok 4.5 Medium do as a worker?

Then I asked Terra: *How'd Grok 4.5 Medium do as a worker?*

> Grok 4.5 Medium is capable, but I'd rate it as a **supervised workhorse** rather than an autonomous one.
>
> In this task it produced a strong first implementation: coherent visual refactor, correct card structure, responsive fixes, and clean archive pages. It also responded well to narrow corrective prompts.
>
> Its weaknesses were operational and security-detail oriented:
>
> - Broad prompts sometimes appeared to complete without a usable report, so I had to split work into smaller slices.
> - It didn't reliably recognize workspace/write boundaries until explicitly probed.
> - Its first "sanitized" Cloudflare pages still included credential-path and token-header examples—technically placeholders, but contrary to the requirement. It fixed them cleanly once the violation was named.
>
> **My practical take:** excellent for fast implementation, refactors, CSS/UI work, and iterative fixes. Use it with explicit acceptance tests, a small initial write probe, and mandatory human/agent review for secrets, deployments, permissions, and edge cases.

## Result

All the changes were made as desired.

I think I'll try **Grok 4.6 High** next time.

I'm not quite sure where I'll eventually draw the line between GPT 5.6 Sol and Terra.

## Subscriptions and orchestration

Oh—I also signed up for the **$20/mo** plan for Cursor to try this out. The **$30/mo Super Grok** was a bust: I got one day's worth of coding with Grok Build and used up **30% of my monthly quota**.

I'm setting up my own orchestrator with roles now. Each role will have a list of coding harnesses/models to use, given a priority and given my limits from subscriptions.
