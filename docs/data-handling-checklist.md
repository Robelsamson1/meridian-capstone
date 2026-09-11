# Data Handling Checklist — Meridian Markets Capstone

## Purpose

This checklist governs how the workshop team handles Meridian Markets data
across all four workshops of this engagement. It applies to both the student
team and Meridian's team (Dana Okafor and colleagues) as a shared reference
for what's safe to do with the data we're given.

It operationalizes the terms in the [client brief](../raw/client-brief.md).
**We have not yet reviewed the NDA.** Items below are marked as either
**[Brief]** — stated explicitly in the client brief — or
**[Pending NDA]** — a recommended control that isn't confirmed by any
document yet and must be checked against the NDA once we've read it. Nothing
in this checklist should be treated as a confirmed client requirement unless
it's marked **[Brief]** or Dana has confirmed it directly.

## Data classification — [Brief]

The client brief states this explicitly and calls it non-negotiable:

> Customer records and employee data do not go into ChatGPT, Claude,
> Copilot, or any other AI tool. That includes the loyalty program data, the
> labor schedules, and any excerpts of them. Our counsel is firm on this,
> and it is not negotiable. Sales totals by store and week, and the store
> attributes, are fine to use with those tools.

**Restricted — never goes into any AI tool, including excerpts:**
- Customer records
- Employee data
- Loyalty program data (membership and purchase history)
- Labor schedules

**Open — the only two data types the brief authorizes for AI-tool use:**
- Sales totals by store and by week
- Store attributes (square footage, opening date, lease terms)

**Everything else is Restricted for AI-tool use by default.** The brief
does not say that aggregating, summarizing, or removing identifiers from
Restricted data changes its classification, and the team should not make
that judgment call on its own. Any data type not on the Open list above —
including raw POS transactions and any aggregated or summarized version of
loyalty or labor data — stays Restricted for AI-tool use until Dana
confirms otherwise. See **Open questions for Dana** below.

## Lifecycle checklist

### Receipt & intake — [Brief]

- [ ] NDA is signed by everyone on the team before requesting any extract
  - *Why: the brief states the NDA will cover everything in it, and access to data follows the signed NDA.*
- [ ] Extract is requested from Marcus (IT) only after the NDA is signed
  - *Why: matches the brief's stated process.*
- [ ] Each file in the extract is classified as Restricted or Open (per the section above) before anyone opens it in a new tool
  - *Why: classification has to happen before use, not after — you can't un-paste data into an AI tool.*

### Storage & access — [Pending NDA]

*The client brief does not specify storage or access controls. The lines
below are recommended practice, not a confirmed client requirement — confirm
against the NDA once we've reviewed it, and update this section accordingly.*

- [ ] Restricted data is stored only in locations accessible to the student team and Meridian, not in general-purpose cloud tools with broader sharing defaults
  - *Why: recommended practice for anything under NDA; to be confirmed against the NDA's actual terms.*
- [ ] No Restricted data is copied into scratch files, notebooks, or chat tools "temporarily" for convenience
  - *Why: temporary copies are a common way sensitive data ends up in the wrong tool.*
- [ ] Access to the raw extract is limited to team members who need it for their current task
  - *Why: reduces the number of places a mistake can happen.*

### AI-tool use — [Brief]

- [ ] Before pasting or uploading anything to an AI tool, confirm it is one of the two explicitly Open data types — sales totals by store/week, or store attributes — and nothing else
  - *Why: the brief authorizes only these two categories for AI-tool use; this is the non-negotiable term.*
- [ ] Do not treat aggregated, summarized, or de-identified versions of customer, loyalty, employee, or labor data as Open, regardless of how the aggregation is done
  - *Why: the brief restricts loyalty data, labor schedules, and "any excerpts of them" outright — it does not carve out an exception for aggregation, and that determination isn't the team's to make.*
- [ ] If a data type isn't explicitly on the Open list, treat it as Restricted for AI-tool use and log it in Open questions for Dana rather than deciding based on team judgment
  - *Why: keeps classification traceable to what the client actually authorized, not to our own inference.*

### Sharing outputs — [Pending NDA]

*Not addressed explicitly in the client brief beyond the AI-tool rule above.
Treat as recommended practice pending NDA review.*

- [ ] Any chart, table, or dashboard shared outside the immediate team (board preview, client deliverables) is checked for whether it exposes Restricted-tier detail (e.g., a chart with too few customers per segment)
  - *Why: outputs can expose sensitive data even when the underlying source never touched an AI tool.*
- [ ] Preliminary materials for the board preview follow the same classification rules as final deliverables
  - *Why: "preliminary" is a timeline distinction, not a data-handling exception.*

### Disposal & retention — [Pending NDA]

*The brief does not specify retention or disposal terms. Do not treat the
items below as confirmed — check them against the NDA once reviewed, and
against whatever Dana/counsel specify directly.*

- [ ] Local copies of the raw extract are deleted or returned per the NDA's terms at the end of the engagement
  - *Why: retention terms are expected to be in the NDA, not the brief — confirm before assuming.*
- [ ] Any Restricted data cached in AI-tool history, browser storage, or local drafts is cleared at project close
  - *Why: closing the loop on Restricted data isn't just about the original file — copies and history count too.*

## Open questions for Dana

The brief authorizes only two data types for AI-tool use (sales totals by
store/week, and store attributes). Everything else below is Restricted for
AI-tool use on our end until you tell us otherwise — we are not assuming
that aggregation or removing identifiers changes that.

- **Raw (unaggregated) POS transaction records** — is this Restricted or Open for AI-tool use?
- **Aggregated/store-level labor data** (e.g., total scheduled hours per store per week, without employee names) — the brief restricts "labor schedules." Does an aggregated, store-level view count as Open, or is it still Restricted?
- **Aggregated loyalty program data** (e.g., average spend per segment, membership counts by store) — the brief restricts "loyalty program data." Does aggregation change that, or is it still Restricted?
- **Storage, access, and disposal/retention terms** — the brief says the NDA covers this; once we've reviewed it, we'll confirm our practices match, but wanted to flag we'll be asking.
