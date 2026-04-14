# Initiative Brief Output Template

This is the reference template for the output format of imported initiative briefs. Each section below represents the ideal structure. When importing, map the user's pasted content into these sections and generate `#QUESTION` tags inline wherever information is missing, vague, or needs PM follow-up.

---

## Problem

What's broken, painful, or unsustainable about how this works today? Who feels it — members, customers, ops staff, finance? What does it cost (in hours, error rates, user experience, vendor spend)?

Describe the current state workflow in enough detail that someone unfamiliar with the system could understand the moving parts, where the manual steps are, and where things break. Call out specific pain points.

### Why now?

What makes this the right time? A vendor deadline, a cost inflection point, a dependency that just got unblocked, a scaling threshold we're about to hit?

---

## Goal & Success Metrics

### Business Outcome

What does success look like for this initiative from a business perspective? Be specific enough that someone could verify it.

### Target Metrics

| Metric | Baseline | Target | How We'll Measure |
|--------|----------|--------|-------------------|
| [Primary impact metric] | | | |
| [Operational cost, e.g. manual labor hrs/week] | | | |
| [Automation rate, e.g. zero-touch %] | | | |
| [Quality signal, e.g. error rate, complaints] | | | |

### Key Operational Signals

A short list of the metrics that tell us whether this initiative is healthy once it's live — proxies for whether the business outcome is being delivered.

- [e.g. Zero-touch rate (daily)]
- [e.g. Queue/backlog depth]
- [e.g. Time from ingestion to completion]
- [e.g. Exception/escalation volume]

---

## Current State *(if already in progress)*

If this initiative has already started or is in production, document where things stand today. Include:

- **Production status:** What's live? What's still in development?
- **Key metrics:** Current performance against targets (e.g., containment rates, automation rates, error rates)
- **What's working:** What's delivering value today?
- **What's not:** Known gaps, blockers, or areas underperforming

Skip this section if the initiative hasn't started yet.

---

## The Lever

What are we building that will deliver the business outcome above? Describe the approach at a high level — what the system will do that it doesn't do today, and what level of automation we're targeting.

This is the "what" and "why this approach," not the "how." Keep it at the level a PM or exec would discuss it, not an engineering design doc.

### First Punch

What's the first vertical slice of delivery that proves the approach works and brings some business value? This isn't a full feature — it's the smallest thing that de-risks the strategy.

---

## Scope

### Must Have

The minimum set of capabilities required for this initiative to deliver its business impact. If any of these are missing, the initiative hasn't shipped.

### Should Have

Capabilities we expect to deliver but could cut under timeline pressure without losing the core value.

### Won't Have

Things someone might reasonably assume are in scope but aren't. Call these out to prevent scope creep and misaligned expectations.

---

## Source of Truth

### What does this system become authoritative for?

| Data / Domain | Current Authority | After This Initiative |
|---------------|-------------------|-----------------------|
| | | |

### Who consumes this data downstream?

Which other systems, teams, or processes depend on this data? This shapes the blast radius of getting it wrong.

---

## Dependencies

### What this initiative needs from other systems

| System | What | When | Status |
|--------|------|------|--------|
| | | | |

### What other systems need from this initiative

| System | What | When |
|--------|------|------|
| | | |

---

## Vendor Replacement *(if applicable)*

| Vendor | Contract End | Annual Cost | What's Being Replaced |
|--------|-------------|-------------|-----------------------|
| | | | |

What must be true before we can turn off the vendor? Will we run both systems in parallel during transition? If so, for how long?

---

## Operational Transition *(if applicable)*

If this initiative changes how ops teams work — retraining, process handoffs, new tools replacing old ones — describe that here. Include the people and teams affected, not just the systems.

---

## Risks

| Risk | Impact | What We'd Do |
|------|--------|--------------|
| | | |
