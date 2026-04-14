---
name: create-product-spec
description: Create a product spec — a lightweight product requirements document for a feature. Use when a PM or engineer wants to write up a feature, create a product spec, spec out a feature, write requirements, or says "create product spec", "create a product spec", "new product spec", "write a spec", "feature spec", "create a PRD", or "I need to spec out X". Generates an interactive, structured product spec in docs/prds/.
---

# Create Product Spec

Generate a structured product spec through an interactive conversation with the user. The product spec should give an AI coding assistant everything it needs to break the feature into tasks and implement it — without prescribing technical implementation or testing strategy.

Product specs live in `docs/prds/` as markdown files.

## Process

### Phase 1: Get the Full Picture

Start by asking the user for a long, detailed description of the problem they want to solve and any initial ideas for solutions. Let them talk. Don't interrupt with structured questions yet — the goal is to get everything out of their head first.

After they've described the feature, assess whether it's too large for a single product spec. If the feature spans multiple independent user roles, separate phases, or distinct subsystems that could be built and shipped independently, suggest breaking it into separate product specs before starting the interview. Use AskUserQuestion to confirm scope with the user.

Then explore the codebase to verify their assertions and understand the current state. Read relevant models, routes, components, and existing docs to ground yourself in what actually exists today. This is important — the user may have outdated assumptions or miss existing functionality that affects scope.

### Phase 2: Challenge the Automation Opportunity

Do this early — before drilling into detailed behaviors — so that scope changes from this conversation don't waste interview time.

Push back on whether the feature is ambitious enough. Many teams default to digitizing existing manual workflows without fundamentally rethinking them.

Ask pointed questions using AskUserQuestion:

1. "Is this feature primarily digitizing an existing manual workflow, or does it fundamentally change how the work gets done?"
2. "What would this look like if no human had to touch it? Where could an agent or automation make decisions autonomously?"
3. "What's the most repetitive or low-judgment part of this workflow that a human currently does?"

If the user's answers suggest the feature is just a digital version of a manual process, respectfully push back:
- Acknowledge the value of the current approach
- Suggest specific areas where automation or AI could reduce human touchpoints
- Ask if any of those automation opportunities should be in scope or flagged as a fast-follow

The goal isn't to block features — it's to make sure the PM has done the thought exercise of "could this be smarter?" before locking in scope.

### Phase 3: Interview Relentlessly

Your mindset for this phase: you are a senior engineer or tech lead sitting across from the PM in a design review. You're mentally planning how you'd build this — thinking about data models, state transitions, API boundaries, edge cases, performance implications, and integration points. But you're NOT writing any of that down in the product spec. Instead, you're asking the product questions that would surface the information an engineer needs to make those technical decisions later.

The key insight: every technical decision depends on a product decision. "Should we use polling or websockets?" depends on "How real-time does the handoff indicator need to be — seconds or minutes?" The engineer figures out the former; the PM needs to answer the latter. Your job is to ask every question like the second one so the product spec contains the answers the implementing engineer (or AI agent) will need.

Drill into details using AskUserQuestion. Walk down each branch of the design tree, resolving dependencies between decisions one by one. Don't move on from a decision point until it's clear.

Batch questions where possible (up to 4 per AskUserQuestion call), but do multiple rounds. Keep going until you've reached a shared understanding of every aspect of the feature. Skip questions you can already answer from the user's initial description or from exploring the codebase — don't re-ask what they've already told you.

**Triage your questions based on feature complexity.** The question areas below are a menu, not a checklist. A simple UI addition might need 2-3 rounds of questions. A feature with new data models, state machines, or external integrations might need 5-8. Use judgment — if a question area doesn't apply, skip it.

After each round of questions, ask the user if there are aspects of the feature you haven't covered yet. When they say no, move on.

**Areas to cover (adapt based on what you already know):**

**Who & Why:**
- Who is the primary user? Are there secondary users?
- What's the triggering situation? When does the user need this?
- What's the desired outcome? What should be true after the feature exists?
- How is this handled today? What's the current workaround or gap?

**Scope & Shape:**
- What's IN scope? Walk through each capability.
- What's explicitly OUT of scope? Name the adjacent things you're NOT building.
- Are there existing features this should resemble or build on?

**Behaviors — dig into each one:**
For each capability the user describes, walk down the decision tree:
- What happens in the happy path?
- What happens when the data doesn't exist yet (empty state)?
- What happens when things go wrong? (Errors, edge cases, permissions)
- Are there states or transitions that need to be explicitly defined?

**Questions a tech lead would ask (phrased as product questions):**
Think about what an engineer implementing this would need to know, and ask the product version of those questions. Pick from this list based on relevance — most features only need a handful of these:
- "How up-to-date does this information need to be? Real-time, or is a few minutes stale acceptable?"
- "How many items could this list have? Tens, hundreds, thousands?"
- "Who can see this? Who can edit it? What does a user without access see?"
- "What are all the possible statuses? Can they go backwards? Who can change them?"
- "Does anyone need to know when this changes? How urgently?"

Don't settle for vague answers. If the user says "the user can filter the list," ask what filters, what the defaults are, whether filters persist, what happens with no results. Resolve ambiguity during the interview, not during implementation.

### Phase 4: Define Metrics

Use a metrics ladder that scales with feature complexity. Ask the user:

1. **Feature metric (leading):** "What changes in the product when this ships? What's the most direct thing you'd measure in the first week?"
   - Get a specific metric, baseline (or "unknown"), and target
   - Example: "Task completion time drops from 5 min to 30 sec"

2. **Product metric (intermediate):** "How does this move a broader product metric?"
   - Only include if the connection is real and measurable
   - Skip for small features where this would be a stretch
   - Example: "Daily case throughput increases"

3. **Business/operational metric (lagging):** "What operational outcome does this ladder up to?"
   - Always include — even small features should connect to business value
   - It's okay if the connection is indirect; name it honestly
   - Example: "Cost per user served decreases"

If the user struggles with metrics, help by suggesting options based on the feature's domain. But don't manufacture fake metrics — if a tier doesn't apply, skip it with a note explaining why.

### Phase 5: Review and Confirm

Before generating, do a quick review pass with the user:

1. **User flow** — If not already clear from the behaviors discussion, ask: "Walk me through what the user does, screen by screen or step by step."

2. **Anything missing?** — "Is there anything about this feature we haven't covered that an engineer would need to know?"

If there are unresolved questions that came up during the interview — things the PM couldn't answer, needs to check with stakeholders, or wants to defer — note them. These will appear inline in the output as `#QUESTION` callouts rather than in a separate section.

### Phase 6: Generate the Product Spec

Check if `docs/prds/` already exists. If so, use it. If not, create it (and `docs/` if needed).

Write the product spec to `docs/prds/<slug>.md` using the template below. The slug should be a kebab-case version of the feature name. If a file with that slug already exists, append the current date (e.g., `copy-buttons-2026-03-22.md`).

After writing, show the user the file path and offer to iterate on any section.

**Handling unresolved items:** Throughout the product spec, anywhere a decision is still open or information is missing, insert an inline callout:

- `> **#QUESTION:** [Question that needs an answer before implementation]`

These should appear right where the missing information matters — inside a behavior's scenario, next to a metric, within the scope section — not collected in a separate section at the bottom. This way an engineer reading the product spec hits the open item exactly when they'd need the answer.

## Output Template

```markdown
# Product Spec: [Feature Name]

**Author:** [PM name if known, otherwise leave blank]
**Date:** [today's date]
**Status:** Draft

**Related docs:**
- [Link to any design docs, overview docs, or TODO docs that informed this product spec]

---

## Problem Statement

[What problem does this solve? Who is affected? Why now? Write this from the user's perspective — describe the pain or gap they experience, not the solution.]

**How it works today:** [Current state / workaround / manual process / gap]

---

## Solution

[High-level description of the proposed solution, from the user's perspective. What will be true after this ships? Keep this to a paragraph — the detail lives in the behaviors section below.]

---

## Scope

### In Scope
- [Capability 1]
- [Capability 2]
- ...

### Out of Scope
- [Thing we're NOT building and why]
- ...

---

## Behaviors

1. **[Behavior name]**

   **When** [triggering situation / context],
   **I want to** [motivation / action],
   **So I can** [expected outcome].

   **Scenario: [Happy path]**
   - Given [precondition]
   - When [action]
   - Then [result]

   **Scenario: [Edge case / error]**
   - Given [precondition]
   - When [action]
   - Then [result]

2. **[Behavior name]**

   **When** [triggering situation / context],
   **I want to** [motivation / action],
   **So I can** [expected outcome].

   **Scenario: [Happy path]**
   - Given [precondition]
   - When [action]
   - Then [result]

*(Continue numbering for every behavior. Aim for completeness — it's better to over-specify than to leave ambiguity for the implementer to guess at.)*

---

## Success Metrics

### Feature Metric (Leading)
- **Metric:** [What you're measuring]
- **Baseline:** [Current state or "TBD"]
- **Target:** [Expected improvement]
- **How to measure:** [Where/how this is tracked]

### Product Metric (Intermediate)
*(Include only when the connection is real and measurable. Omit for small features.)*
- **Metric:** [Broader product metric this moves]
- **Connection:** [How the feature metric drives this]

### Business Metric (Lagging)
- **Metric:** [Operational outcome]
- **Connection:** [How this feature contributes]

---

## User Flow

1. [Step 1 — what the user sees/does]
2. [Step 2]
3. ...

---

## Automation Opportunity

[Notes from the automation challenge conversation. What aspects of this workflow could be automated or made smarter in the future? Where are the human-in-the-loop touchpoints that could eventually be reduced?]
```

## Style Guidelines

- Write in plain language. No jargon, no acronyms without definition.
- The behaviors list should be extensive and numbered. Cover every aspect of the feature — happy paths, edge cases, empty states, permissions, error handling. If a behavior feels obvious, include it anyway. An AI coding assistant reading this list should be able to enumerate every task without guessing.
- Each behavior combines the job story (why) with BDD scenarios (what). The job story provides context; the scenarios provide precision.
- Metrics should be honest — don't force a metric that doesn't make sense. It's better to say "No meaningful intermediate metric for a feature this size" than to invent one.
- The product spec should contain NO implementation decisions (which API, which component, which database pattern) and NO testing decisions. Those belong to the engineer. The product spec defines what the product does, not how it's built.
- The product spec should be complete enough that an AI coding assistant could read it and produce a detailed implementation plan without asking clarifying questions about product requirements.
- Use `#QUESTION` callouts inline wherever information is missing or decisions are unresolved. Don't collect these into a separate section — put them where the gap is so the reader encounters them in context.
- Keep the tone professional but not corporate. This is a working document, not a board presentation.
