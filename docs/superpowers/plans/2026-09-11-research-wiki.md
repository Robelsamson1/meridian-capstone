# Research Wiki Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Stand up the initial research wiki (`wiki/` + root `CLAUDE.md`) from `raw/client-brief.md`, producing a ready-to-use interview agenda for the Dana Okafor stakeholder interview.

**Architecture:** Follows the raw/wiki/schema pattern: `raw/client-brief.md` (untouched source) → `wiki/sources/`, `wiki/entities/`, `wiki/concepts/`, `wiki/analyses/` (LLM-authored pages, cross-linked with `[[wikilinks]]`) → `wiki/index.md` / `wiki/log.md` / `wiki/overview.md` (catalog, history, synthesis). `CLAUDE.md` at repo root is the schema doc future sessions read automatically.

**Tech Stack:** Plain markdown files, git. No scripts, no automation — this is a manual content-authoring pass.

**Spec:** `docs/superpowers/specs/2026-09-11-research-wiki-design.md`

**Note on verification steps:** This plan produces markdown content, not executable code, so each task's "test" is a content-presence check (via `grep`/`cat`) rather than a unit test — the same red/green discipline (verify before, verify after) applied to prose deliverables.

## Global Constraints

- File naming: kebab-case, `.md` extension. No front matter.
- Every wiki page starts with a `Source:` line near the top citing the raw source(s) or wiki page(s) it's derived from.
- Wiki-internal citations use `[[page-name]]` syntax (path relative to `wiki/`, no extension, e.g. `[[entities/dana-okafor]]`). Links to files outside `wiki/` (e.g. `docs/`, `raw/`) use a plain backtick path or normal markdown link, never `[[ ]]` syntax.
- Data-safety rule: no wiki page may ever contain actual values from Restricted data (customer records, employee data, loyalty program data, labor schedules) — only structural descriptions (counts, date ranges, field names). Any data type not confirmed Open defaults to Restricted and gets logged in `wiki/analyses/open-questions.md`, never guessed into a page.
- No ingest tooling/automation is in scope — every file below is hand-authored content.
- Every task ends with a git commit of exactly the files it created/modified.

---

### Task 1: Schema doc (`CLAUDE.md`)

**Files:**
- Create: `CLAUDE.md`

**Interfaces:**
- Produces: the schema/conventions doc every later task and every future session reads first. No dependency on other tasks.

- [x] **Step 1: Write `CLAUDE.md`**

```markdown
# Meridian Capstone — Wiki Schema

This file defines how the research wiki in `wiki/` is built and maintained
for the Meridian Markets capstone engagement. Read this before creating or
editing any wiki page. Full rationale: `docs/superpowers/specs/2026-09-11-research-wiki-design.md`.

## Layers

1. `raw/` — immutable source documents. Never edited once added.
2. `wiki/` — LLM-authored markdown: summaries, entity pages, concept pages,
   analyses. This is the layer this file governs.
3. This file — schema, conventions, and the ingest workflow.

## Directory structure

```
wiki/
  index.md        # catalog of all wiki pages, one-line summaries, by category
  log.md          # append-only chronological record of ingests/updates
  overview.md     # synthesis of key themes across the engagement
  sources/        # one page per document in raw/
  entities/       # people, organizations, named things
  concepts/       # recurring ideas, methods, themes
  analyses/       # syntheses, comparisons, investigations, open questions
```

## Page conventions

- File naming: kebab-case, `.md` extension, one page per distinct
  entity/concept/source/analysis.
- No front matter.
- Every page starts with a `Source:` line citing what it's derived from,
  e.g. `Source: [[sources/client-brief]]`.
- Wiki-internal links use `[[page-name]]` (path relative to `wiki/`, no
  extension). A link to a page that doesn't exist yet is fine — it marks a
  page worth writing later, not an error. Links to files outside `wiki/`
  (e.g. `docs/`, `raw/`) use a plain path, not `[[ ]]` syntax.

## Data-safety rule (binding on every wiki page)

Per the client brief's non-negotiable terms (see
`docs/data-handling-checklist.md`): customer records, employee data,
loyalty program data, and labor schedules — including any excerpts — must
never enter an AI tool. Since this wiki is LLM-authored:

- Pages may describe a Restricted source only structurally — row counts,
  date ranges, field/column names, high-level shape — never actual values.
- Sales totals by store/week and store attributes (square footage, opening
  date, lease terms) are the only data types authorized as Open. Everything
  else defaults to Restricted.
- If it's unclear whether something is Restricted, treat it as Restricted
  and add a question to `wiki/analyses/open-questions.md` rather than
  guessing.

## Ingest workflow

1. Read the new raw source in `raw/`.
2. Apply the data-safety rule above to classify what can be captured.
3. Create or update the relevant `sources/`, `entities/`, `concepts/`, and
   `analyses/` pages, with `[[wikilink]]` citations both ways.
4. Update `wiki/index.md` with any new/changed pages and one-line summaries.
5. Append a dated entry to `wiki/log.md` describing what was ingested.

A source in `raw/` with no corresponding `wiki/sources/*.md` page is
"pending ingest" — this is a computed state, not tracked separately.
```

- [x] **Step 2: Verify required sections are present**

Run: `grep -c "^## " CLAUDE.md`
Expected: `6` (Layers, Directory structure, Page conventions, Data-safety rule, Ingest workflow, plus one — confirm the count matches the number of `##` headings actually written; re-check by eye if the count differs, since heading wording may shift slightly during authoring)

Run: `grep -n "Restricted" CLAUDE.md`
Expected: at least one match, confirming the data-safety rule is present.

- [x] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "Add wiki schema doc (CLAUDE.md)

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `CLAUDE.md` exists at repo root with the six sections above.
**How you check it:** Open `CLAUDE.md` and confirm it covers layers, directory structure, page conventions, the data-safety rule, and the ingest workflow — this is the doc that governs every other task in this plan.

---

### Task 2: Source page (`wiki/sources/client-brief.md`)

**Files:**
- Create: `wiki/sources/client-brief.md`

**Interfaces:**
- Consumes: `raw/client-brief.md` (already in repo).
- Produces: `[[sources/client-brief]]`, the citation target every entity/concept page in this plan links back to.

- [x] **Step 1: Write `wiki/sources/client-brief.md`**

```markdown
# Client brief: Meridian Markets

Source: `raw/client-brief.md` (email from Dana Okafor, VP of Operations, to
the LMU MSBA workshop team, August 2026)

## Summary

Dana Okafor is asking the workshop team to build a store/category sales
dashboard to support Meridian Markets' next expansion decision, with a
preliminary board presentation due in three weeks.

## Key facts

- [[entities/meridian-markets]]: specialty grocery chain, 14 stores across
  Los Angeles, Orange, and Ventura counties. ~$78M annual revenue, ~620
  employees.
- Grew from 6 to 14 stores in five years by taking over leases vacated by
  chains exiting neighborhoods Meridian considered underserved. See
  [[concepts/store-expansion-strategy]].
- Competes on prepared foods, local sourcing, and a smaller footprint than
  national chains. See [[concepts/prepared-foods-positioning]].
- Requested deliverable: a dashboard showing sales performance by store and
  by category, to support a decision on a Pasadena location.
- Growth has been uneven across stores — some took off immediately, others
  slower.
- Loyalty program has ~40,000 members; the data hasn't been meaningfully
  analyzed to date. See [[concepts/loyalty-program]].
- Data available: ~3 years of POS transactions, loyalty membership/purchase
  history, labor scheduling/hours, store attributes (square footage,
  opening date, lease terms).
- Meridian migrated to a new POS system in spring 2026.
- Data access requires a signed NDA; extract requested from Marcus (IT)
  afterward.
- Non-negotiable data rule: customer records, employee data, loyalty data,
  and labor schedules — including excerpts — may never go into any AI
  tool. Sales totals by store/week and store attributes are the only types
  authorized for AI-tool use. See `docs/data-handling-checklist.md`.
- Timeline: ~8 week engagement; a preliminary board preview is needed
  within 3 weeks.
- Contact: [[entities/dana-okafor]].
```

- [x] **Step 2: Verify citation and wikilinks are present**

Run: `grep -n "^Source:" wiki/sources/client-brief.md`
Expected: one match, `Source: \`raw/client-brief.md\`...`

Run: `grep -oE '\[\[[a-z/-]+\]\]' wiki/sources/client-brief.md | sort -u`
Expected output includes: `[[concepts/loyalty-program]]`, `[[concepts/prepared-foods-positioning]]`, `[[concepts/store-expansion-strategy]]`, `[[entities/dana-okafor]]`, `[[entities/meridian-markets]]`

- [x] **Step 3: Commit**

```bash
git add wiki/sources/client-brief.md
git commit -m "Add source page for client brief

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/sources/client-brief.md` exists, starts with a `Source:` line, and links to all five entity/concept pages this plan creates.
**How you check it:** Run the two grep commands above; both should match as described.

---

### Task 3: Entity page (`wiki/entities/dana-okafor.md`)

**Files:**
- Create: `wiki/entities/dana-okafor.md`

**Interfaces:**
- Consumes: `[[sources/client-brief]]` (Task 2).
- Produces: `[[entities/dana-okafor]]`, referenced by Tasks 8 and 9.

- [x] **Step 1: Write `wiki/entities/dana-okafor.md`**

```markdown
# Dana Okafor

Source: [[sources/client-brief]]

## Role

VP of Operations at [[entities/meridian-markets]]. Author of the client
brief that starts this engagement (August 2026).

## What she wants

- A dashboard showing sales performance by store and category, to support
  a decision on opening a Pasadena location. See
  [[concepts/store-expansion-strategy]].
- A better understanding of the loyalty program's ~40,000 members, which
  she says the company has "never really used." See
  [[concepts/loyalty-program]].
- Overall: increased revenue, reduced operating costs, and improved
  customer experience, driven by better visibility into store/category
  performance.

## Working style & logistics

- Email is the best way to reach her.
- Travels Tuesdays and Wednesdays; slow to reply — silence isn't a sign of
  a problem.
- Her assistant can schedule time but cannot answer analytics questions.
- Wants something to show the board when it meets in three weeks, even if
  preliminary.

## Open questions

See [[analyses/open-questions]] and [[analyses/interview-prep]].
```

- [x] **Step 2: Verify citation and wikilinks are present**

Run: `grep -n "^Source:" wiki/entities/dana-okafor.md`
Expected: `Source: [[sources/client-brief]]`

Run: `grep -c '\[\[' wiki/entities/dana-okafor.md`
Expected: `5` or more (links to meridian-markets, store-expansion-strategy, loyalty-program, open-questions, interview-prep)

- [x] **Step 3: Commit**

```bash
git add wiki/entities/dana-okafor.md
git commit -m "Add entity page for Dana Okafor

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/entities/dana-okafor.md` exists, cites the client brief, and captures her role, goals, and logistics.
**How you check it:** Open the file — confirm the three sections (Role, What she wants, Working style & logistics) and both grep checks pass.

---

### Task 4: Entity page (`wiki/entities/meridian-markets.md`)

**Files:**
- Create: `wiki/entities/meridian-markets.md`

**Interfaces:**
- Consumes: `[[sources/client-brief]]` (Task 2).
- Produces: `[[entities/meridian-markets]]`, referenced by Tasks 2, 3, 5, 6, 7, 9.

- [x] **Step 1: Write `wiki/entities/meridian-markets.md`**

```markdown
# Meridian Markets

Source: [[sources/client-brief]]

## Overview

Specialty grocery chain with 14 stores across Los Angeles, Orange, and
Ventura counties. Roughly $78M in annual revenue and about 620 employees.

## Positioning

Competes against national chains on prepared foods, local sourcing, and a
smaller store footprint. See [[concepts/prepared-foods-positioning]].

## Growth history

Grew from 6 stores to 14 in five years, mostly by taking over leases from
chains that pulled out of neighborhoods Meridian considered underserved.
Growth has been uneven — some stores took off immediately, others have
been slower to find their footing. See
[[concepts/store-expansion-strategy]].

## Systems & data

- Migrated to a new POS system in spring 2026.
- Runs a loyalty program with about 40,000 members. See
  [[concepts/loyalty-program]].
- Holds ~3 years of POS transaction history, loyalty data, labor
  scheduling/hours, and store attributes (square footage, opening date,
  lease terms).

## Key contact

[[entities/dana-okafor]], VP of Operations.
```

- [x] **Step 2: Verify citation and wikilinks are present**

Run: `grep -n "^Source:" wiki/entities/meridian-markets.md`
Expected: `Source: [[sources/client-brief]]`

Run: `grep -c '\[\[' wiki/entities/meridian-markets.md`
Expected: `4` or more

- [x] **Step 3: Commit**

```bash
git add wiki/entities/meridian-markets.md
git commit -m "Add entity page for Meridian Markets

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/entities/meridian-markets.md` exists with overview, positioning, growth history, systems/data, and key contact sections.
**How you check it:** Open the file and confirm all five sections are present; both grep checks pass.

---

### Task 5: Concept page (`wiki/concepts/prepared-foods-positioning.md`)

**Files:**
- Create: `wiki/concepts/prepared-foods-positioning.md`

**Interfaces:**
- Consumes: `[[sources/client-brief]]`, `[[entities/meridian-markets]]` (Tasks 2, 4).
- Produces: `[[concepts/prepared-foods-positioning]]`, referenced by Tasks 2, 3, 4, 9.

- [x] **Step 1: Write `wiki/concepts/prepared-foods-positioning.md`**

```markdown
# Prepared foods positioning

Source: [[sources/client-brief]]

## What it is

One of [[entities/meridian-markets]]'s three stated points of competitive
differentiation against national grocery chains: prepared foods, local
sourcing, and a smaller store footprint.

## Why it matters for this engagement

The requested dashboard breaks sales down "by store and by category" —
prepared foods is a named category worth tracking specifically, since it's
part of how Meridian describes its own competitive edge, not just another
line item.

## Open questions

The brief doesn't say how large prepared foods is as a share of revenue,
or how it performs relative to other categories. See
[[analyses/open-questions]].
```

- [x] **Step 2: Verify citation and wikilinks are present**

Run: `grep -n "^Source:" wiki/concepts/prepared-foods-positioning.md`
Expected: `Source: [[sources/client-brief]]`

Run: `grep -c '\[\[' wiki/concepts/prepared-foods-positioning.md`
Expected: `2` or more

- [x] **Step 3: Commit**

```bash
git add wiki/concepts/prepared-foods-positioning.md
git commit -m "Add concept page for prepared foods positioning

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/concepts/prepared-foods-positioning.md` exists, cites the client brief, and links to `entities/meridian-markets` and `analyses/open-questions`.
**How you check it:** Open the file and confirm both grep checks pass.

---

### Task 6: Concept page (`wiki/concepts/loyalty-program.md`)

**Files:**
- Create: `wiki/concepts/loyalty-program.md`

**Interfaces:**
- Consumes: `[[sources/client-brief]]`, `[[entities/meridian-markets]]` (Tasks 2, 4).
- Produces: `[[concepts/loyalty-program]]`, referenced by Tasks 2, 3, 4, 9.

- [x] **Step 1: Write `wiki/concepts/loyalty-program.md`**

```markdown
# Loyalty program

Source: [[sources/client-brief]]

## What it is

[[entities/meridian-markets]]'s customer loyalty program, with roughly
40,000 members. It captures membership and purchase history.

## Status

Dana Okafor states the company doesn't think it has "ever really used"
this data, despite wanting to "understand our customer better."

## Data handling

Loyalty program data (membership and purchase history) is classified
**Restricted** for AI-tool use per the client brief's non-negotiable
terms — it and any excerpts of it may never be pasted into an AI tool. See
`docs/data-handling-checklist.md`. Per `CLAUDE.md`, any wiki page
describing loyalty data may only describe it structurally (e.g., member
count, that purchase history exists) — never actual values.

## Open questions

See [[analyses/open-questions]] — including whether aggregated loyalty
metrics (e.g., average spend per segment) are Restricted or Open.
```

- [x] **Step 2: Verify citation, wikilinks, and the data-safety note are present**

Run: `grep -n "^Source:" wiki/concepts/loyalty-program.md`
Expected: `Source: [[sources/client-brief]]`

Run: `grep -n "Restricted" wiki/concepts/loyalty-program.md`
Expected: at least one match, confirming the data-handling note is present.

- [x] **Step 3: Commit**

```bash
git add wiki/concepts/loyalty-program.md
git commit -m "Add concept page for loyalty program

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/concepts/loyalty-program.md` exists, cites the client brief, and explicitly states its Restricted data-handling status.
**How you check it:** Open the file and confirm the "Data handling" section names the Restricted classification; both grep checks pass.

---

### Task 7: Concept page (`wiki/concepts/store-expansion-strategy.md`)

**Files:**
- Create: `wiki/concepts/store-expansion-strategy.md`

**Interfaces:**
- Consumes: `[[sources/client-brief]]`, `[[entities/meridian-markets]]` (Tasks 2, 4).
- Produces: `[[concepts/store-expansion-strategy]]`, referenced by Tasks 2, 3, 4, 9.

- [x] **Step 1: Write `wiki/concepts/store-expansion-strategy.md`**

```markdown
# Store expansion strategy

Source: [[sources/client-brief]]

## History

[[entities/meridian-markets]] grew from 6 stores to 14 in five years,
primarily by taking over leases from chains that exited neighborhoods
Meridian considered underserved.

## Current decision

Leadership sees a Pasadena site as "the obvious next step" but wants
sales/category data to confirm the decision before committing. This is the
direct motivation for the requested dashboard.

## Complication

Growth hasn't been even — some of the 14 stores took off immediately,
others have been slower to find their footing. Understanding what
separates the two groups is likely central to both the dashboard and the
Pasadena decision.

## Relevant data

Store attributes (square footage, opening date, lease terms) are
classified **Open** for AI-tool use per the client brief, unlike most other
data types. See `docs/data-handling-checklist.md`.

## Open questions

See [[analyses/open-questions]] and [[analyses/interview-prep]].
```

- [x] **Step 2: Verify citation and wikilinks are present**

Run: `grep -n "^Source:" wiki/concepts/store-expansion-strategy.md`
Expected: `Source: [[sources/client-brief]]`

Run: `grep -c '\[\[' wiki/concepts/store-expansion-strategy.md`
Expected: `4` or more

- [x] **Step 3: Commit**

```bash
git add wiki/concepts/store-expansion-strategy.md
git commit -m "Add concept page for store expansion strategy

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/concepts/store-expansion-strategy.md` exists covering history, the current Pasadena decision, the uneven-growth complication, and which related data is Open.
**How you check it:** Open the file and confirm all four sections are present; both grep checks pass.

---

### Task 8: Open questions (`wiki/analyses/open-questions.md`) + checklist edit

**Files:**
- Create: `wiki/analyses/open-questions.md`
- Modify: `docs/data-handling-checklist.md` (replace the "## Open questions for Dana" section)

**Interfaces:**
- Consumes: `[[concepts/loyalty-program]]`, `[[concepts/store-expansion-strategy]]`, `[[concepts/prepared-foods-positioning]]`, `[[sources/client-brief]]` (Tasks 2, 5, 6, 7); the 4 existing questions currently in `docs/data-handling-checklist.md`.
- Produces: `[[analyses/open-questions]]`, referenced by Task 9 and by every entity/concept page above.

- [x] **Step 1: Write `wiki/analyses/open-questions.md`**

```markdown
# Open questions for Dana

Living list of open questions for Dana Okafor. This is the single source
of truth — `docs/data-handling-checklist.md` links here instead of keeping
its own copy.

## Data classification

Source: `docs/data-handling-checklist.md`, [[concepts/loyalty-program]]

- Raw (unaggregated) POS transaction records — Restricted or Open for
  AI-tool use?
- Aggregated/store-level labor data (e.g., total scheduled hours per store
  per week, without employee names) — does aggregation move it out of
  "labor schedules," or is it still Restricted?
- Aggregated loyalty program data (e.g., average spend per segment,
  membership counts by store) — does aggregation move it out of "loyalty
  program data," or is it still Restricted?
- Storage, access, and disposal/retention terms — expected to be covered
  by the NDA; to be confirmed once reviewed.

## Business context

Source: [[sources/client-brief]], [[concepts/store-expansion-strategy]],
[[concepts/prepared-foods-positioning]]

- What specifically makes the Pasadena site "the obvious next step" — an
  available lease, demographics, competitor presence, something else?
- How does leadership define an "underserved neighborhood" when
  evaluating a new site?
- Of the 14 stores, which took off immediately and which were slower —
  and does Dana have a working theory for why?
- What does "success" look like concretely for the board preview in three
  weeks — a specific chart, a specific number, a go/no-go recommendation
  on Pasadena?
- What's the current status of the NDA, and is there a target date for
  Marcus (IT) to pull the data extract?
- Is prepared foods tracked as its own category in the POS system, or
  would that require additional mapping?
```

- [x] **Step 2: Verify both sections and the migrated items are present**

Run: `grep -c "^## " wiki/analyses/open-questions.md`
Expected: `2` (Data classification, Business context)

Run: `grep -c "^- " wiki/analyses/open-questions.md`
Expected: `10` (4 data-classification questions + 6 business-context questions)

- [x] **Step 3: Replace the open-questions section in the checklist**

In `docs/data-handling-checklist.md`, replace this entire block (currently
the last section of the file, starting at `## Open questions for Dana`):

```markdown
## Open questions for Dana

The brief authorizes only two data types for AI-tool use (sales totals by
store/week, and store attributes). Everything else below is Restricted for
AI-tool use on our end until you tell us otherwise — we are not assuming
that aggregation or removing identifiers changes that.

- **Raw (unaggregated) POS transaction records** — is this Restricted or Open for AI-tool use?
- **Aggregated/store-level labor data** (e.g., total scheduled hours per store per week, without employee names) — the brief restricts "labor schedules." Does an aggregated, store-level view count as Open, or is it still Restricted?
- **Aggregated loyalty program data** (e.g., average spend per segment, membership counts by store) — the brief restricts "loyalty program data." Does aggregation change that, or is it still Restricted?
- **Storage, access, and disposal/retention terms** — the brief says the NDA covers this; once we've reviewed it, we'll confirm our practices match, but wanted to flag we'll be asking.
```

with:

```markdown
## Open questions for Dana

Tracked in the research wiki: see
[wiki/analyses/open-questions.md](../wiki/analyses/open-questions.md).
```

- [x] **Step 4: Verify the checklist no longer duplicates the list**

Run: `grep -c "Raw (unaggregated) POS" docs/data-handling-checklist.md`
Expected: `0` (the duplicated question text is gone)

Run: `grep -n "wiki/analyses/open-questions.md" docs/data-handling-checklist.md`
Expected: one match, the new pointer link.

- [x] **Step 5: Commit**

```bash
git add wiki/analyses/open-questions.md docs/data-handling-checklist.md
git commit -m "Add open-questions analysis page; checklist links to it

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/analyses/open-questions.md` exists with both the migrated data-classification questions and new business-context questions; `docs/data-handling-checklist.md` no longer contains its own copy of the questions and instead links to the wiki page.
**How you check it:** Run all four grep commands above; each should match as described. Open both files to confirm nothing was lost in the migration.

---

### Task 9: Interview prep (`wiki/analyses/interview-prep.md`)

**Files:**
- Create: `wiki/analyses/interview-prep.md`

**Interfaces:**
- Consumes: `[[entities/dana-okafor]]`, `[[entities/meridian-markets]]`, `[[concepts/store-expansion-strategy]]`, `[[concepts/prepared-foods-positioning]]`, `[[concepts/loyalty-program]]`, `[[analyses/open-questions]]` (Tasks 3, 4, 5, 6, 7, 8).
- Produces: `[[analyses/interview-prep]]`, the main deliverable of this pass — referenced by Task 10's index.

- [x] **Step 1: Write `wiki/analyses/interview-prep.md`**

```markdown
# Interview prep: Dana Okafor

Source: [[entities/dana-okafor]], [[entities/meridian-markets]],
[[concepts/store-expansion-strategy]], [[concepts/prepared-foods-positioning]],
[[concepts/loyalty-program]], [[analyses/open-questions]]

Ready-to-use agenda for the stakeholder interview with Dana Okafor, VP of
Operations.

## Logistics reminders

- Reach her by email; she travels Tuesdays and Wednesdays and is slow to
  reply.
- Her assistant can schedule time but cannot answer analytics questions —
  route substantive questions to Dana directly.
- She wants something to show the board in three weeks, even if
  preliminary — confirm what "something" means concretely early in the
  interview.

## Agenda

### 1. Company context & growth

- Of the 14 stores, which took off immediately and which were slower to
  find their footing?
- What's your working theory for the difference between the fast and slow
  stores?

### 2. Pasadena / expansion decision

- What specifically makes Pasadena "the obvious next step" today?
- How do you define an "underserved neighborhood" when evaluating a site?
- What would change your mind about Pasadena, if anything?

### 3. Categories & prepared foods

- How is "category" currently defined in your systems — is prepared foods
  its own tracked category?
- Which categories, besides prepared foods, do you see as most important
  to track by store?

### 4. Customer & loyalty

- What would "understanding the customer better" actually let you do
  differently?
- Has anyone looked at loyalty data before, even informally?

### 5. Data & systems

- What changed with the new POS system since spring 2026 — new fields, new
  categories, any gaps in the historical data around the migration?
- Who besides Marcus should we loop in on the data extract?

### 6. Success criteria & timeline

- What does the three-week board preview need to include, at minimum?
- Beyond revenue/costs/customer experience, are there other success
  metrics leadership is tracking?

### 7. Data handling / NDA

- What's the current status of the NDA?
- Confirm the data-classification open questions in
  [[analyses/open-questions]] — particularly whether aggregated
  loyalty/labor data can be treated as Open.
```

- [x] **Step 2: Verify all seven agenda sections and the citation are present**

Run: `grep -n "^Source:" wiki/analyses/interview-prep.md`
Expected: one match listing all six cited pages.

Run: `grep -c "^### " wiki/analyses/interview-prep.md`
Expected: `7`

- [x] **Step 3: Commit**

```bash
git add wiki/analyses/interview-prep.md
git commit -m "Add interview-prep agenda for Dana Okafor stakeholder interview

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/analyses/interview-prep.md` exists with logistics reminders and a 7-section agenda covering growth, expansion, categories, loyalty, systems, success criteria, and data handling.
**How you check it:** Run both grep commands above; read through the agenda and confirm you'd be comfortable walking into the interview with it.

---

### Task 10: Index, log, overview

**Files:**
- Create: `wiki/index.md`
- Create: `wiki/log.md`
- Create: `wiki/overview.md`

**Interfaces:**
- Consumes: every page produced in Tasks 2–9.
- Produces: the three wiki-wide pages that close out this ingest pass.

- [x] **Step 1: Write `wiki/index.md`**

```markdown
# Wiki index

Catalog of all wiki pages. See `CLAUDE.md` for conventions and
`docs/superpowers/specs/2026-09-11-research-wiki-design.md` for the design
spec.

## sources/

- [[sources/client-brief]] — Dana Okafor's engagement kickoff email:
  company overview, requested dashboard, data available, data-handling
  terms, timeline.

## entities/

- [[entities/dana-okafor]] — VP of Operations, Meridian Markets; the
  primary stakeholder contact.
- [[entities/meridian-markets]] — the client: a 14-store specialty grocery
  chain across LA/Orange/Ventura counties.

## concepts/

- [[concepts/prepared-foods-positioning]] — Meridian's prepared-foods/
  local-sourcing/small-footprint competitive positioning.
- [[concepts/loyalty-program]] — the ~40,000-member loyalty program and its
  Restricted data-handling status.
- [[concepts/store-expansion-strategy]] — Meridian's lease-takeover growth
  history and the Pasadena decision.

## analyses/

- [[analyses/open-questions]] — the living list of open questions for Dana
  (data classification + business context).
- [[analyses/interview-prep]] — the ready-to-use interview agenda for the
  Dana Okafor stakeholder interview.
```

- [x] **Step 2: Write `wiki/log.md`**

```markdown
# Wiki log

Append-only. Newest entries at the bottom.

## 2026-09-11 — Initial ingest: client brief

Ingested `raw/client-brief.md`. Created `sources/client-brief.md`,
`entities/dana-okafor.md`, `entities/meridian-markets.md`,
`concepts/prepared-foods-positioning.md`, `concepts/loyalty-program.md`,
`concepts/store-expansion-strategy.md`, `analyses/open-questions.md`,
`analyses/interview-prep.md`, `index.md`, and this log. Also added
`CLAUDE.md` at repo root and updated `docs/data-handling-checklist.md` to
link to `analyses/open-questions.md` instead of embedding its own copy. No
Restricted data was ingested — the client brief is the only source, and
it's not customer, employee, loyalty, or labor data.
```

- [x] **Step 3: Write `wiki/overview.md`**

```markdown
# Overview

Synthesis of the engagement so far, based on [[sources/client-brief]].

[[entities/meridian-markets]] is a 14-store specialty grocery chain that
grew quickly (6 to 14 stores in five years) by taking over leases in
neighborhoods larger chains left. That growth has been uneven, and
leadership — represented by [[entities/dana-okafor]] — wants data-backed
visibility into store and category performance before deciding on the next
site, widely assumed to be Pasadena. See
[[concepts/store-expansion-strategy]].

Two threads run through the brief beyond the headline dashboard ask: the
company's competitive identity is built on
[[concepts/prepared-foods-positioning]], and there's an acknowledged,
unexploited data asset in the [[concepts/loyalty-program]]'s ~40,000
members. Both are candidates for deeper analysis once the underlying data
is available.

A hard constraint shapes how this team can work: customer, employee,
loyalty, and labor data can never touch an AI tool, in any form — only
sales totals by store/week and store attributes are cleared for that. This
wiki's own schema (`CLAUDE.md`) enforces that boundary on every page.

The immediate pressure is timeline: an ~8-week engagement with a
preliminary board preview needed in 3 weeks, well before the full data
extract and analysis can be complete. [[analyses/interview-prep]] is built
with that constraint in mind — get the highest-value context from Dana
now, while the NDA and data extract are still in progress.
```

- [x] **Step 4: Verify no source is missing from the index and every wikilink target exists**

Run:
```bash
for f in wiki/sources/*.md wiki/entities/*.md wiki/concepts/*.md wiki/analyses/*.md; do
  grep -q "$(basename "$f" .md)" wiki/index.md || echo "MISSING FROM INDEX: $f"
done
```
Expected: no output (every page is listed in the index).

Run:
```bash
grep -ohE '\[\[[a-z/-]+\]\]' wiki/*.md wiki/*/*.md | tr -d '[]' | sort -u | while read -r target; do
  [ -f "wiki/$target.md" ] || echo "DANGLING LINK: $target"
done
```
Expected: no output (every `[[wikilink]]` in the wiki resolves to a real file — none are expected to dangle, since this is a closed first pass with no pages left un-created).

- [x] **Step 5: Commit**

```bash
git add wiki/index.md wiki/log.md wiki/overview.md
git commit -m "Add wiki index, log, and overview

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01T9eZvM5nj7UcD4rNBZSzoz"
```

**Done looks like:** `wiki/index.md` lists all 8 content pages by category; `wiki/log.md` has one dated entry describing this ingest; `wiki/overview.md` synthesizes the growth story, the two under-explored threads (prepared foods, loyalty), the data-safety constraint, and the timeline pressure.
**How you check it:** Run both verification scripts in Step 4 — both should print nothing. Then read `wiki/overview.md` once and confirm it reads as a coherent two-minute briefing on the engagement.

---

## Final check across the whole plan

- [x] Every file listed in the spec's "Initial content" section exists: `CLAUDE.md`, `wiki/index.md`, `wiki/log.md`, `wiki/overview.md`, `wiki/sources/client-brief.md`, `wiki/entities/dana-okafor.md`, `wiki/entities/meridian-markets.md`, `wiki/concepts/prepared-foods-positioning.md`, `wiki/concepts/loyalty-program.md`, `wiki/concepts/store-expansion-strategy.md`, `wiki/analyses/open-questions.md`, `wiki/analyses/interview-prep.md`, and the edit to `docs/data-handling-checklist.md`.
- [x] `git log --oneline -10` shows one commit per task (10 commits total), each scoped to only the files that task touched.
