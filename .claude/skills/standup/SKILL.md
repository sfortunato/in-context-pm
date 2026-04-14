---
name: standup
description: Process standup or meeting notes — extract decisions and scope changes, propose updates to product specs and initiative docs. Use when the user says "standup", "standup notes", "meeting notes", "process meeting notes", "capture decisions", or pastes meeting notes and wants them turned into doc updates.
---

# Standup

Extract decisions, scope changes, blockers, and action items from meeting notes and propose updates to the relevant planning docs (product specs, initiative docs, backlogs). Most standups produce zero doc changes — this skill exits gracefully when there's nothing actionable.

## Phase 1: Ingest Notes

Ask the user to paste or point to meeting notes:

> "Paste the meeting notes below, or give me a file path. I'll extract decisions and scope changes that should flow back into planning docs."

Accept:
- Pasted text in the conversation
- A file path to read

## Phase 2: Extract

Parse the notes and categorize into:

1. **Decisions** — things that were agreed upon that affect product scope, technical direction, or priorities. Examples: "We decided to descope X", "PM confirmed Y behavior", "We'll use approach Z for the integration"

2. **Scope changes** — explicit additions, removals, or deferrals to existing features or product specs. Examples: "Moving feature X to phase 2", "Adding support for Y", "Dropping the admin view for now"

3. **Blockers** — new blockers or updates to existing blockers. Examples: "Waiting on external API", "Staging environment down", "Need design for the approval flow"

4. **Action items** — who owes what, with any mentioned deadlines. Examples: "Jeremy to share the schema by Friday", "Manny to confirm pricing rules"

Not every standup has all four categories. Many have none — that's fine.

Present the extraction to the user using AskUserQuestion:
- "I extracted the following from the notes. Is this accurate? Should I add, remove, or rephrase anything?"
- Show each category with bullet points

If nothing actionable was found, say so: "No decisions or scope changes found in these notes. The standup was mostly status updates." Then stop — don't force doc updates.

## Phase 3: Propose Updates

For each **decision** and **scope change**, scan for related planning docs that should be updated:

1. Look in `docs/prds/` for product specs matching keywords from the decisions
2. Look for initiative docs in `docs/initiatives/`
3. Look for backlog docs in `docs/initiatives/`

For each relevant doc, propose a specific edit:
- Show the file path
- Show the existing text that would change (or where new text would be inserted)
- Show the proposed new text
- Explain why: "This decision means <X>, so the product spec should reflect <Y>"

Use AskUserQuestion for each proposed update:
- "Should I apply this change to `<file>`?"
- Options: Apply, Skip, Edit (let me rephrase)

If a decision doesn't map to any existing doc, note it: "This decision doesn't match any existing product spec or initiative doc. Consider capturing it when you next run `/product-spec-create` or `/initiative-breakdown`."

## Phase 4: Apply

Only apply changes the user explicitly approved. Use the Edit tool for surgical updates — don't rewrite entire files.

After all changes, summarize:
- Which files were updated
- Which decisions were captured
- Which items were skipped

If blockers or action items were extracted but don't map to doc changes, list them as a reference: "These items don't require doc updates but may be worth tracking elsewhere."

## Guidelines

- **Be conservative.** Only propose doc updates for clear decisions and scope changes. Status updates ("I'm working on X") are not doc changes.
- **Be surgical.** Edit the specific paragraph or bullet that's affected. Don't restructure documents.
- **Respect #QUESTION callouts.** If a decision resolves an existing `#QUESTION` in a product spec, propose removing the callout and replacing it with the decided answer.
- **Don't create new docs.** This skill updates existing planning docs. If the user needs a new product spec, point them to `/product-spec-create`.
- **Handle sensitive info carefully.** Meeting notes may contain names, IDs, or sensitive information. Never write sensitive data into planning docs. If you detect potential sensitive info in the notes, flag it and exclude it from any proposed doc updates.
