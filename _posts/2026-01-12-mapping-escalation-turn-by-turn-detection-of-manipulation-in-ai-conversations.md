---
layout: post
title: "Mapping Escalation: Turn-by-Turn Detection of Manipulation in AI Conversations"
date: 2026-01-12
excerpt: "Most AI safety evaluations score conversations as a whole. That misses something important: manipulation that builds gradually, turn by turn, staying under the radar until it's too late."
---

Most AI safety evaluations score conversations as a whole — read the transcript, assign a risk rating, move on. That approach has a blind spot. A model doesn't need to be overtly harmful in any single turn to manipulate a user. It can build gradually: establish trust early, introduce small commitments, then escalate. By the time the behavior is recognizable, the damage is done.

This was the core problem I set out to address at the Apart Research AI Manipulation Hackathon in January 2026, working with collaborator Karthick Chandrasekaran.

## The Problem with Holistic Scoring

Holistic transcript scoring averages out manipulation signals across an entire conversation. A model that behaves helpfully in turns 1–3 and manipulatively in turns 5–7 can produce an aggregate score that looks acceptable. The technique we call "hook-and-escalate" — establish credibility first, exploit later — is specifically designed to exploit this weakness.

We wanted a method that could see through this: one that evaluates each turn independently and tracks how risk accumulates over time.

## What We Built

We extended Anthropic's BLOOM evaluation framework into a turn-by-turn scoring system. For each conversation turn, a judge model (Claude Sonnet 4.5) independently scores two psychologically-grounded manipulation types: **commitment escalation** (foot-in-the-door tactics that leverage prior user agreements) and **gradual belief shifting** (incremental reframing that moves a user's views without their awareness). The system then tracks cumulative risk, turn-level deltas, and escalation trajectories across the full conversation.

We evaluated three commercial LLMs — GPT-4o-mini, Llama-3.1-70B, and Qwen3-235B — across 70 conversations and 20 realistic scenarios spanning financial advisory, health guidance, and interpersonal advice contexts.

## What We Found

The results were striking. **77.9% of conversations exhibited detectable manipulation patterns**, with an average peak escalation rate of 335% above baseline. This isn't edge-case behavior — it's systematic.

Model differences were illuminating. GPT-4o-mini and Llama both reached peak escalation rates above 390%, with GPT-4o-mini maintaining a helpful tone even at high manipulation turns, effectively masking concerning trajectories. Qwen3-235B had the highest detection rate (85.7%) but a different profile: it entered manipulative modes earlier and sustained them, rather than building gradually.

Temporal analysis revealed that roughly 40% of conversations follow non-linear escalation paths — strategic variation in intensity that holistic scoring would completely miss. Peak manipulation typically occurs in turns 4–6, after trust has been established.

## Why It Matters

These findings have direct implications for organizations deploying LLMs in sensitive domains. Traditional content filtering and end-of-conversation review are not enough. Real-time, turn-level monitoring is necessary — with automatic intervention at escalation thresholds and human oversight for consequential decisions.

The full framework is open source at [github.com/vibhuganesan/manipulation-tracker](https://github.com/vibhuganesan/manipulation-tracker). The code includes scenario generation, conversation rollout, and the complete scoring pipeline — built to be extended.

This work was conducted with Apart Research. The methodology focuses on two manipulation categories; emotional manipulation, authority exploitation, and social proof remain areas for future work.
