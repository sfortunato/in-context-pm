---
name: initiative-import
description: Import an initiative brief into the project through an interactive interview. Use when the user says "import initiative", "initiative import", "add initiative", "paste initiative brief", or wants to bring an initiative brief into the repo.
---

# Import Initiative

Import an initiative brief into `docs/initiatives/<slug>.md` through an interactive interview.

**Core principle:** Assume whoever runs this skill is the domain expert — DRI, PM, or stakeholder. Interview them to extract what they know. Don't silently write `#QUESTION` tags for gaps the user could answer right now if asked.

## Output Format

Read the reference template at `./TEMPLATE.md` (relative to this skill file). This defines the ideal section structure and serves as the interview roadmap — each section's guidance text tells you what to ask about.

When building the final document:

- **Map gathered information into the template sections.** Restructure by meaning, not exact headings.
- **Generate `#QUESTION` tags inline** only for things the user explicitly deferred or couldn't answer. These are last-resort markers, not the default for gaps.
- **Drop sections that don't apply** (e.g. Vendor Replacement if no vendor is involved). Don't leave empty sections.
- **Preserve the user's language** where possible — restructure, don't rewrite.

## Process

### Phase 1: Identify the Initiative

Ask the user what initiative they're importing. Get:
- A name/title for the initiative
- Which service or project area it belongs to (if applicable)

This determines the output file slug (e.g., `docs/initiatives/user-onboarding.md`).

### Phase 2: Ingest the Raw Content

Accept either:
- A file path (from `$ARGUMENTS` or user message) to read from
- Pasted text in the conversation

**If a file path is provided, read it directly.** For `.docx` files, use `pandoc <file> -o /tmp/initiative-import.md` then read the markdown output. For other file types, read directly. Do NOT ask the user to paste content if they already gave you a file.

If neither a file path nor pasted text is provided, ask the user:

> "Paste the initiative brief below, or give me a file path. If it's in Google Docs, copy the full content — I can't access GDocs directly."

Read the content along with the output template from `./TEMPLATE.md`.

Identify what's filled vs. what's placeholder/empty in the input.

### Phase 3: Acknowledge What We Have

Read `./TEMPLATE.md` and classify each section against what the input provides:

| Status | Meaning |
|--------|---------|
| **FILLED** | Input covers this well enough to write the section |
| **PARTIAL** | Input has something but needs clarification or detail |
| **EMPTY** | Nothing in the input maps to this section |

Show the user this gap map as a quick table before interviewing. This grounds the conversation and focuses the interview on what's actually missing.

### Phase 4: Interview to Fill Gaps

Walk through each template section that is PARTIAL or EMPTY. Use AskUserQuestion popups — batch up to 4 related questions per popup. Do multiple rounds.

**Early detection — is this already in production?** Ask upfront: "Is any part of this initiative already live in production?" If yes, pivot the interview: focus on current metrics, remaining gaps, migration/maintenance work, and team transition plans rather than greenfield scoping. Ask "What's the current state?" before "What will you build?"

**Interview order follows the template, with recommended questions per topic:**

1. **Problem & Why Now** — "What's the current workflow and where does it break?" "What's the forcing function — a contract date, cost threshold, or scaling limit?"
2. **Goal & Success Metrics** — "What are the baseline numbers today?" "What's the target, and how will you measure it?" Push for specific numbers, not qualitative goals.
3. **Current State** *(if already in progress)* — "What's live today and what are the current metrics?" "What's working well vs. underperforming?"
4. **The Lever** — "What are you building that you think will deliver the outcome?" If the answer is vague, ask: "Is the engineering work mostly configuring a vendor, building APIs, or both?"
5. **First Punch** — "What's the smallest thing you could ship that proves this works?" If already shipped, ask for current metrics and reframe the section as "Already delivered — here's where we are."
6. **Scope** — "What are the 3-4 must-haves without which this initiative hasn't shipped?" "What might people assume is in scope but isn't?"
7. **Source of Truth** — "What data does this system become authoritative for?" "Who consumes it downstream?"
8. **Dependencies** — "What other systems or teams does this depend on?" "Are there blocking dependencies with deadlines?"
9. **Vendor Replacement** — "Is this replacing a vendor, adding to the stack, or neither?" If replacing: "When does the contract renew and what's the annual cost?"
10. **Operational Transition** — "Does this change how any ops team works day-to-day?" If yes: "Who needs retraining and what's the timeline?"
11. **Risks** — "What's the biggest thing that could go wrong?" "What would you do if [key dependency] slips?"

**Guidelines:**
- **Skip sections already FILLED** by the input. Don't re-ask what's already there.
- **Provide recommended answers or options** where you can infer from the input content or reasonable defaults. Let the user confirm or correct.
- **Don't settle for vague answers.** If the user says "we'll automate most of it," ask what "most" means — which types, what percentage, what's the exception path?
- **After each round, ask if there's anything you haven't covered.** When the user says they're done or wants to move on, stop.
- **Respect "I don't know" and "TBD."** Mark those as `#QUESTION` in the output — that's what the tags are for. Don't push if the user genuinely doesn't have the answer yet.

### Phase 5: Challenge Round

One focused AskUserQuestion round to push on ambition. Ask:

1. "Is this initiative primarily moving work from [vendor/manual process] to a new system, or does it fundamentally change how the work gets done?"
2. "What would this look like if no human had to touch it? Where could automation make decisions autonomously?"

If the user's answers suggest the initiative is just digitizing an existing workflow, respectfully note where automation or AI could reduce human touchpoints. Don't block — just make sure the thought exercise happened.

Skip this phase if the input already addresses automation level clearly.

### Phase 6: Build & Write the Document

Create `docs/initiatives/` if it doesn't exist.

Build the document:

1. **Prepend a metadata header:**

```markdown
# Initiative: <Initiative Name>

> <one-line tagline>

| Field | Value |
|-------|-------|
| **DRI** | <if known, otherwise #QUESTION> |
| **Target Date** | <if known, otherwise #QUESTION> |
| **Success Metric** | <primary metric> |

---
```

2. **Map all gathered information into the template structure.** Use everything from the original input plus the interview answers. `#QUESTION` tags only for items the user explicitly deferred.

3. **Write to** `docs/initiatives/<slug>.md`.

If a file already exists at that path, use AskUserQuestion:
- "An initiative doc already exists at this path. Overwrite it, or append the new content below the existing?"
- Options: Overwrite, Append, Cancel

### Phase 7: Confirm and Next Steps

Tell the user:
- The file path that was created
- "Run `/initiative-breakdown` to generate a product spec backlog from this initiative"
- "Run `/product-spec-create` to start fleshing out individual features"
