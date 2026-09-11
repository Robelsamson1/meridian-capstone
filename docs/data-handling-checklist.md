# Data Handling Checklist — Meridian Markets Capstone

## Purpose

This checklist governs how the workshop team handles Meridian Markets data
across all four workshops of this engagement. It applies to both the student
team and Meridian's team (Dana Okafor and colleagues) as a shared reference
for what's safe to do with the data we're given.

It operationalizes the terms in the [client brief](../raw/client-bried.md)
and the NDA that follows it. Where this checklist and the NDA disagree, the
NDA governs — flag the conflict and ask Dana rather than guessing.

## Data classification

Per the brief, data falls into two tiers:

**Restricted — never goes into ChatGPT, Claude, Copilot, or any other AI
tool, including excerpts.**
- Loyalty program membership and purchase history (the ~40,000-member data)
- Labor scheduling and hours
- Any customer or employee record, or any derivative of one that could
  re-identify a specific person or employee

**Open — safe to use with AI tools.**
- Sales totals by store and by week
- Store attributes (square footage, opening date, lease terms)

Some data types fall between these two categories and aren't explicitly
classified by the brief — see **Open questions for Dana** below. Treat
anything not clearly Open as Restricted until confirmed.

## Lifecycle checklist

### Receipt & intake

- [ ] NDA is signed by everyone on the team before requesting any extract
  - *Why: the brief is explicit that the NDA must be in place first, and it covers everything in the brief.*
- [ ] Extract is requested from Marcus (IT) only after the NDA is signed
  - *Why: matches the brief's stated process — asking early risks getting data outside the agreed terms.*
- [ ] Each file in the extract is classified as Restricted or Open before anyone opens it in a new tool
  - *Why: classification has to happen before use, not after — you can't un-paste data into an AI tool.*

### Storage & access

- [ ] Restricted data is stored only in locations accessible to the student team and Meridian, not in general-purpose cloud tools with broader sharing defaults
  - *Why: the NDA's protection depends on controlled access, not just AI-tool exclusion.*
- [ ] No Restricted data is copied into scratch files, notebooks, or chat tools "temporarily" for convenience
  - *Why: temporary copies are the most common way sensitive data leaks into the wrong tool.*
- [ ] Access to the raw extract is limited to team members who need it for their current task
  - *Why: reduces the number of places a mistake can happen.*

### AI-tool use

- [ ] Before pasting or uploading anything to an AI tool, confirm it's Open-tier, not just "probably fine"
  - *Why: this is the brief's one non-negotiable term — counsel is firm on it.*
- [ ] Aggregated or summarized outputs derived from Restricted data are re-checked to confirm the aggregation actually removes identifying detail before treating them as Open
  - *Why: a "summary" can still leak individual-level detail if the aggregation is too fine-grained (e.g., a single-customer segment).*
- [ ] When in doubt whether a dataset is Open, default to not using it with AI tools and ask
  - *Why: the cost of asking is small; the cost of a data leak under a signed NDA is not.*

### Sharing outputs

- [ ] Any chart, table, or dashboard shared outside the immediate team (board preview, client deliverables) is checked for whether it exposes Restricted-tier detail (e.g., a chart with too few customers per segment)
  - *Why: outputs can leak sensitive data even when the underlying source never touched an AI tool.*
- [ ] Preliminary materials for the board preview follow the same classification rules as final deliverables
  - *Why: "preliminary" doesn't loosen the NDA's terms.*

### Disposal & retention

- [ ] Local copies of the raw extract are deleted or returned per the NDA's terms at the end of the engagement
  - *Why: confirm the exact terms with Dana/counsel — the brief doesn't specify retention, so don't assume.*
- [ ] Any Restricted data cached in AI-tool history, browser storage, or local drafts is cleared at project close
  - *Why: closing the loop on Restricted data isn't just about the original file — copies and history count too.*

## Open questions for Dana

The brief's two-tier split doesn't explicitly cover every data type we expect
to work with. Please confirm how the following should be classified before we
treat them as Open:

- **Raw (unaggregated) POS transaction records** — no customer identifiers, but transaction-level. Is this Open, or does it need aggregation first?
- **Store-level labor hours without employee names** (e.g., total scheduled hours per store per week) — is this still Restricted as "labor scheduling and hours," or does removing names make it Open?
- **Loyalty program aggregates** (e.g., average spend per segment, membership counts by store) — at what level of aggregation, if any, does loyalty data become safe to use with AI tools?
