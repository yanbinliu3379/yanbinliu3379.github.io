---
title: "Internal Knowledge as a Service"
description: "Most organizations have their standards written down and still don't follow them. That isn't a discipline problem — it's an operating model problem."
date: 2026-08-23
author: Yanbin Liu
layout: post
tags: [ai-agents, mcp, knowledge-management, enterprise-architecture, governance]
---

# Internal Knowledge as a Service

## TL;DR

- **Written standards don't enforce themselves.** Whether one gets followed depends on somebody remembering it exists, finding the current version, and choosing to apply it — three chances to fail before any work starts.
- **The cost lands late.** Review gates, rework, and inconsistent output, caught after the expensive part is already done.
- **AI changes that for an unglamorous reason: it reads.** An assistant that consults the standard on every request turns a document nobody opens into something that actually governs the work.
- **Control stays with the business.** The rules stay where they are today, owned and edited by the same people. Changing one means editing a page — no developer, no release, no ticket.
- **You find out what's missing.** An incomplete request comes back as a specific list of gaps, rather than a plausible-looking document with a hole in it that nobody thinks to question.
- **What it won't do.** It applies your standard; it can't fix a bad one. It only sees the systems it is connected to. And it suits a small, well-kept knowledge base rather than a sprawling one.

## A knowledge base can't enforce anything

Every organization has its standards written down. Almost none of them get followed consistently, and that isn't a discipline problem.

A document can only ever suggest. It can't announce that it exists. It can't tell you it changed last week. It can't stop a specification being written that ignores it, and it can't distinguish between someone who read it and someone who didn't. Compliance depends on a person remembering the knowledge exists, finding the current version, and choosing to apply it — three chances to fail before any work starts.

So we compensate afterwards. Review gates. Checklists. Templates copied once and then quietly diverging. A platform team catching problems late enough that fixing them is expensive.

The knowledge itself is rarely the gap. It's in the templates people copy, the naming conventions nobody can quite recite, the decision records, the runbooks, the segmentation rules, the one paragraph in a wiki that turns out to be the real contract everyone works to. It exists. It has simply never been in the room at the moment the work happens.

## How far does the standard actually reach?

Before anything else, it helps to know where you are. Almost every organization sits somewhere on the same ladder, and the rung is decided by one question — not what tooling you own, but how close the standard gets to the moment work happens.

```mermaid
flowchart TB
    L0["0 — Written down<br/>The standard exists. Nobody reads it."]
    L1["1 — Checked afterwards<br/>Review gates catch the drift, late and expensively."]
    L2["2 — Templated<br/>Copied once at the start, then quietly diverges."]
    L3["3 — Read at the moment of work<br/>Consulted on every request, not from memory."]
    L4["4 — Enforced<br/>The output is checked against the current standard, deterministically."]
    L5["5 — Recorded<br/>Every application traceable to the version that governed it."]

    L0 --> L1 --> L2 --> L3 --> L4 --> L5

    classDef gap fill:#F1F3F6,stroke:#5B6478,color:#1E2235
    classDef turn fill:#FFF7ED,stroke:#EA580C,color:#1E2235
    classDef goal fill:#F0FDFA,stroke:#0F766E,color:#1E2235
    class L0,L1,L2 gap
    class L3 turn
    class L4,L5 goal
```

Levels 0 to 2 differ in effort, not in kind. The standard is consulted before the work or after it — never during.

That is the whole gap. Everything from level 3 onward needs something in the room at the moment the work happens, checking against the current version rather than a remembered one. Until recently that meant a person, which is why most organizations stop at 2 and compensate with review gates.

The rest of this is about getting from 2 to 4.

## What's missing isn't context. It's an operating model for it

When an agent does the drafting, something changes — not because the model is clever, but because it reads. A standard consulted at the moment of work, on every request, without depending on anyone's memory or goodwill, is a different kind of object from a standard sitting on a wiki. It stops being a reference and becomes a control point — and that is available to any process whose standard is already written down.

What it takes is a controlled way to put proprietary knowledge in front of an agent, evidence that the agent actually used it, and an arrangement plain enough that security and platform owners will put their name to it.

Internal knowledge as a service, in other words: published rather than copied, owned by someone, versioned, and reachable in its current form at the moment it matters.

## Three ways to close the gap

```mermaid
flowchart LR
    subgraph SK["Static KB"]
        direction TB
        S0["Business user"]
        S1["1 — Written standard<br/>sits on the wiki, advisory only"]
        S2["2 — Depends on memory<br/>must be found, read, applied"]
        S3["3 — Drift, unnoticed<br/>review gates catch it late"]
        S4(["Inconsistent output<br/>best for reference material only"])
        S0 --> S1 --> S2 --> S3 --> S4
    end

    subgraph RG["Bespoke RAG — pre-MCP"]
        direction TB
        R0["Engineering team"]
        R1["1 — Crawl and chunk<br/>every document split into passages"]
        R2["2 — Embed and index<br/>vector DB, scheduled re-index"]
        R3["3 — Retrieve top-k<br/>process hard-coded in a system prompt"]
        R4(["Retrieved passage<br/>best for large sprawling corpora"])
        R0 --> R1 --> R2 --> R3 --> R4
    end

    subgraph MS["MCP + Skill — this post"]
        direction TB
        M0["Business owner edits directly"]
        M1["1 — Live connector<br/>MCP reads the knowledge on every call"]
        M2["2 — Skill holds process<br/>versioned prose, editable same day"]
        M3["3 — Deterministic check<br/>complete, or exactly what is missing"]
        M4(["Compliant artifact<br/>best for a small governed corpus"])
        M0 --> M1 --> M2 --> M3 --> M4
    end

    SK ~~~ RG
    RG ~~~ MS

    classDef stale fill:#F1F3F6,stroke:#5B6478,color:#1E2235
    classDef pipe fill:#EEEDFC,stroke:#4F46E5,color:#1E2235
    classDef live fill:#FDF1E4,stroke:#EA580C,color:#1E2235
    class S0,S1,S2,S3,S4 stale
    class R0,R1,R2,R3,R4 pipe
    class M0,M1,M2,M3,M4 live
    style SK fill:#FBFCFD,stroke:#5B6478,stroke-width:2px
    style RG fill:#FBFBFF,stroke:#4F46E5,stroke-width:2px
    style MS fill:#FFFCF8,stroke:#EA580C,stroke-width:2px
```

*Same goal, three approaches — what changed isn't the ambition, it's who ends up owning the copy of the knowledge.*

There are only really three, and the difference between them isn't ambition. It's who ends up owning the copy of the knowledge.

**The static knowledge base** is column one — where most organizations are today, and where this piece started. The standard sits on the wiki, advisory only, and everything downstream depends on someone remembering it exists. Fine for reference material. Not fine for anything that needs enforcing.

**The bespoke pipeline** is what closing that gap used to require, before MCP existed. Crawl the space, chunk every document into passages, embed them, store them in a vector database, schedule a re-index, retrieve top-k at query time, and hard-code the process into a system prompt. Then hand-write the integrations and build a front end, because there was no host application to plug into. Months of engineering, a team, and infrastructure to run indefinitely.

Effort isn't the difference that matters, though. **That system owns a copy of the knowledge**, and everything follows from it: drift between source and index, re-index lag, and chunking that destroys the structure you depend on — a table row holding a pick-list does not survive being cut into passages. What comes back is a retrieved passage, not the current standard. The control plane sits with engineering too, so changing a question means a commit, a review and a deploy.

None of which makes that approach wrong. It is the right answer to a different question — a large, sprawling corpus where you cannot know in advance which document matters, and a team with the budget and headcount to run it.

**The connector and the skill** is column three, and it is what the rest of this describes.

## How this one actually works

```mermaid
flowchart TB
    U["BUSINESS USER<br/>describes the problem in a paragraph<br/>no template, no platform vocabulary"]
    AG["AGENT ENGINE — probabilistic<br/>runs the interview, drafts the output<br/>reaches nothing except through the boundary"]

    subgraph HL["HARNESS LAYER — the control boundary"]
        direction LR
        SK["SKILL — the process, in prose<br/>what to ask, what never to guess, when to escalate<br/>owner: the process owner, changes rarely"]
        MS["MCP SERVER — deterministic code<br/>live fetch, approved options, schema validation<br/>placement check, write-back"]
    end

    subgraph BD["OUTSIDE THE BOUNDARY"]
        direction LR
        W["KNOWLEDGE — the wiki you already have<br/>templates, rules, naming, guardrails<br/>owner: the business, changes weekly"]
        SR["SYSTEMS OF RECORD<br/>published request, test ticket<br/>implementation register"]
    end

    U -->|"asks"| AG
    AG -.->|"follows the process"| SK
    AG <==>|"tool calls, validated responses"| MS
    MS -->|"reads live, nothing copied"| W
    MS ==>|"writes back"| SR

    classDef person fill:#EFF6FF,stroke:#1D4ED8,color:#111827
    classDef eng fill:#FFF7ED,stroke:#EA580C,color:#111827
    classDef harness fill:#F0FDFA,stroke:#0F766E,color:#111827
    classDef store fill:#FFFFFF,stroke:#1D4ED8,color:#111827
    classDef rec fill:#FAFAF9,stroke:#A8A29E,color:#111827
    class U person
    class AG eng
    class SK,MS harness
    class W store
    class SR rec
    style HL fill:#FAFFFE,stroke:#0F766E,stroke-width:3px
    style BD fill:#FCFCFC,stroke:#A8A29E,stroke-width:2px,stroke-dasharray: 6 4
```

*The knowledge base sits outside the harness. The connector and the skill are what wrap the model.*

The model sits outside that boundary and reaches nothing except through it. Everything it is allowed to know about how we work crosses the line in one direction, and everything it produces crosses back in the other — checked on the way. The model reasons but never holds a standard. The connector fetches and validates but holds no rules of its own. The skill decides the shape of the conversation. Move any one of those three across the line and the guarantees change.

Three layers, each with one job and one owner.

**The knowledge stays where it is** — on the wiki the business already maintains. Nothing is copied, nothing is indexed. It could be Confluence, SharePoint, a docs repo. What matters is that someone owns it.

**A connector reads it live, and checks the result against it.** An MCP server, holding no rules of its own: it fetches whatever the standard says today, offers only the answers that standard allows, then checks the finished draft against the same source. The model runs the interview; the connector decides whether the result passes. Compliance isn't something an agent can talk its way into.

**A skill holds the process** — what to ask, what must never be guessed, when to stop and escalate.

Keeping those last two apart is the point. Knowledge changes weekly and belongs to the business; process changes rarely and belongs to whoever runs it. Split that way, each is updated by the person who owns it.

What that buys:

- **Rules change without a release.** We needed a new mandatory field, answered from a fixed list. I added one row to a table in the knowledge base. On the next run the agent asked the new question, in the business's own wording, and flagged the earlier submissions that were now incomplete. No code, no deployment, no ticket.
- **Nothing goes stale.** There's no second copy to drift, so the standard being applied is always the one currently published.
- **Failure is visible.** An incomplete request comes back as a list of what's missing, not a confident document with a hole in it.
- **The requester needs to know nothing.** They describe what they need in plain language, answer a few questions offering options rather than a blank form, and the finished artifact lands where it belongs — complete, checked, and recorded.

## Why it holds up

A model on its own answers once and stops. Wrapped in the connector, the skill and the live knowledge, it becomes a **harness** — the control layer deciding what to call, in what order, and whether what comes back is trustworthy enough to hand to a person. And the reason that beats a well-crafted prompt is the loop, not the prompt. It thinks, calls a tool, observes what came back, and thinks again, until the request is complete or there is a specific reason to escalate. A chatbot answers. A harness keeps working until the standard is satisfied — and every loop still passes the same deterministic check, so more reasoning doesn't buy a way around it.

Two objections land straight away. **If editing the knowledge changes what the system does, who controls it?** The same permissions and version history that already govern the standard, owned by the business rather than an IT backlog. **And if the wiki is unreachable?** Reads are cached, and when the standard can't be read the agent stops rather than guesses. The failure mode is refusal, not confident improvisation.

## Where it runs out — and the layer you add next

This works because the knowledge base is small and well kept — dozens of governed items, not thousands. Grow it well past that and exact fetch stops being enough on its own.

The instinct is to read that as a trade-off: direct fetch *or* search. It isn't. They answer different questions, and a mature setup runs both.

**Layer 1 — direct fetch.** For anything with a shape: templates, forms, pick-lists, schemas, approved option lists, the current version of a named rule. No chunking, no embedding, no relevance scoring. You ask for a specific thing and get that thing, whole and current. This is where determinism lives, and it is the only layer a compliance check can be built on.

**Layer 2 — search, called as a tool.** For anything without a shape: historical cases, prior decisions, sprawling documentation, *has anyone dealt with this before*. The agent reaches for it when it needs context rather than a rule — and it doesn't matter that retrieval is approximate, because nothing is being validated against the result.

The part that matters is that **the connector orchestrates both**. Search becomes another tool call behind the same boundary, subject to the same checks, rather than a second architecture bolted alongside the first. Adding Layer 2 doesn't undo Layer 1 — it stops Layer 1 being asked to do a job it was never suited for.

I have only built Layer 1. Layer 2 is where this goes when the corpus outgrows it, and I would rather say that plainly than describe a system larger than the one actually running.

Two things it doesn't do. It only sees the systems it's connected to, so it will never tell you that no conflict exists — it tells you what it checked and what it couldn't. And it applies your standard rather than inventing one: a contradictory standard produces bad work faster.

## The agent was never the point

None of this is clever. A wiki, a connector, a written process, and a harness holding them together so a model can keep working instead of answering once. The interesting part was realizing the knowledge was always the asset, and the agent only ever the thing reading it.

Treat internal knowledge as a service and the agent becomes the least interesting part of the system. That was the surprise.
