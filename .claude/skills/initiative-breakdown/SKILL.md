---
name: initiative-breakdown
description: Break an initiative into an ordered product spec backlog with dependency sequencing and automation challenges. Use when the user says "initiative breakdown", "breakdown initiative", "break down initiative", "initiative to product specs", "plan initiative", "what product specs should we build", "product spec backlog", or wants to decompose an initiative brief into buildable features.
---

# Breakdown Initiative

Read an initiative brief and break it into an ordered backlog of recommended product specs. Each product spec is scoped as a vertical slice of work that a PM can flesh out with `/create-product-spec`. The ordering follows critical path logic: dependency-unblocked first, then highest impact, then lowest risk.

Your role here is **acting tech lead** — you're the person who reads the business goals, understands the technical landscape, and figures out how to sequence the work so a small team can ship incrementally with the most value delivered earliest.

## Phase 1: Load Context

Read all of these before asking any questions. You need the full picture.

### Initiative brief
Look for the initiative doc:
1. `docs/initiatives/` directory — find the relevant file
2. If no initiative doc exists, ask the user to paste it or run `/create-initiative` first

### Codebase exploration
Explore the codebase to understand:
- Current architecture and tech stack
- Existing models, routes, and components relevant to the initiative
- Integration points with other systems
- Any existing docs in `docs/` that provide context

### Dependencies
Look for any dependency documentation in the repo. Check:
- `docs/dependencies.md` or similar
- README sections about integrations
- Existing API clients or service connections in the codebase

Build two lists:
1. **This project depends on** — external APIs, services, or systems it needs
2. **Other systems depend on this** — APIs or data this project must expose

## Phase 2: Present the Landscape

Before interviewing, present a concise summary so the user sees what you see:

**Project overview:** What this project does, who owns it, what the initiative aims to achieve.

**Dependency map** (if any found):
- Inbound (needs from others): list each with status if known
- Outbound (provides to others): list each with status if known
- Flag any dependencies with tight deadlines as urgent

**Initiative goals:** Key goals from the initiative brief

## Phase 3: Interview (AskUserQuestion, multiple rounds)

Use AskUserQuestion for all interview rounds. Batch up to 4 questions per popup. Do multiple rounds — don't try to cram everything into one.

### Round 1 — Scope & Priorities
- "Which goals from the initiative are highest priority for the next milestone?"
- "Are there goals listed that are actually deferred or out of scope for now?"
- "What's the biggest risk to hitting the target date?"
- "Are there capabilities you expect that the initiative doesn't mention?"

### Round 2 — Innovation Challenge

This is critical. The biggest failure mode is rebuilding what an existing system already does instead of building something fundamentally better. Challenge this directly.

For each major capability in the initiative:
- "For <capability>, are we digitizing what the current system does, or fundamentally changing how the work gets done?"
- "What would <capability> look like if no human had to touch it? Where could an agent or automation make decisions autonomously?"
- "What's the most repetitive or low-judgment part of <capability> that a human currently does?"

If answers suggest the initiative is mostly a rebuild, respectfully push back:
- Acknowledge the value of replacing the existing approach
- Suggest specific areas where automation could reduce human touchpoints
- Ask if those automation opportunities should be in scope for a product spec or flagged as fast-follows

### Round 3 — Dependencies & Sequencing
For each **pending** inbound dependency:
- "Is <dependency> truly blocking, or can you ship a version that works around it?"
- If there's a workaround: "Is the workaround sufficient to ship, or do you need the real integration first?"

For each **pending** outbound dependency:
- "Other systems need <thing> from you by <deadline>. Is this on your radar? Does it affect product spec ordering?"

Ask: "Are there any dependencies not documented that you know about?"

### Additional Rounds
Continue as needed based on answers. Adapt — don't ask questions the user already answered. When the user confirms there's nothing else to cover, move on.

## Phase 4: Generate Product Spec Backlog

Based on everything gathered, produce an ordered list of recommended product specs. For each:

- **Title** — descriptive, scoped (e.g., "Inbound Claim Ingestion" not "Claims Phase 1")
- **Brief scope** — 2-3 sentences describing what this product spec covers
- **Initiative goals served** — which goals from the initiative doc this addresses
- **Dependencies** — flagged with status and deadline if known
- **Sequencing rationale** — why this product spec comes at this position in the order
- **Automation opportunity** — specific notes on where AI/automation could replace human workflow in this feature area

**Ordering logic:**
1. Dependencies-first: product specs that unblock other work come first
2. Highest impact: product specs that move the success metric the most
3. Lowest risk: When impact is similar, prefer the product spec with fewer unknowns
4. Group related capabilities when they share dependencies or data models

## Phase 5: Iterate

Present the backlog to the user. Use AskUserQuestion:
- "Does this ordering make sense? Should anything move up or down?"
- "Should any of these product specs be merged (too granular) or split (too large)?"
- "Are there product specs missing that you expected to see?"
- "Any title or scope adjustments?"

Iterate until the user approves.

## Phase 6: Write the Backlog

Write to `docs/initiatives/backlog.md` (or `docs/initiatives/<slug>-backlog.md` if there are multiple initiatives). Create the directory if needed.

Use this template:

```markdown
# Product Spec Backlog: <Initiative Name>

> Source: <link to initiative doc>
> Generated: <date>
> Target date: <from initiative doc if known>
> Success metric: <from initiative doc>

## Summary

<1-2 paragraph overview: what this project does, the overall sequencing strategy, and the key dependencies that drive the ordering.>

## Dependencies Overview

### This project needs:
| From | What | Deadline | Status |
|------|------|----------|--------|
| ... | ... | ... | ... |

### Other systems need from this:
| To | What | Deadline | Status |
|----|------|----------|--------|
| ... | ... | ... | ... |

---

## 1. <Product Spec Title>

**Scope:** <2-3 sentences>

**Goals served:** <from initiative>

**Dependencies:**
- <flagged dependencies with status>

**Automation opportunity:** <specific notes>

**Next step:** Run `/create-product-spec` to flesh this out

---

## 2. <Product Spec Title>

...

<!-- Continue for each recommended product spec -->
```

After writing, tell the user:
- The file path
- Suggest running `/create-product-spec` for the first product spec in the backlog
- Remind them that `/standup` can capture scope changes back into this doc as work progresses
