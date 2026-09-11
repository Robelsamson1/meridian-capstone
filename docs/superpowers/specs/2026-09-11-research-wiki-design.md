# Research wiki design — Meridian Markets capstone

## Purpose

A persistent research wiki for the full four-workshop capstone engagement,
following the raw/wiki/schema pattern described in
[Karpathy's LLM wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).
Its first concrete use is preparing for the stakeholder interview with Dana
Okafor (VP of Operations), but it is not a single-purpose interview doc — it
is meant to keep accumulating sources, entities, concepts, and analysis
across the whole engagement.

Non-goal: this spec does not decide how the eventual dashboard or data
analysis is built. It only covers the research/knowledge wiki used to
prepare for interviews and track what the team has learned.

## Layers

1. **`raw/`** — immutable source documents, untouched by the LLM beyond
   reading. Exists today with `client-brief.md`. Future additions (NDA,
   interview notes/transcripts, POS/loyalty/labor extract *documentation*)
   land here as they arrive.
2. **`wiki/`** — LLM-authored markdown: summaries, entity pages, concept
   pages, analyses. This is the layer defined in detail below.
3. **`CLAUDE.md`** (repo root) — the schema/config doc. Defines conventions,
   the ingest workflow, and the data-safety rule, and is auto-loaded by
   future Claude sessions working in this repo (per the design discussion,
   the root `CLAUDE.md` location was chosen over a plain `docs/` file so the
   conventions are actually enforced going forward, not just documented).

## Directory structure

```
wiki/
  index.md                  # catalog of all wiki pages, one-line summaries, by category
  log.md                    # append-only chronological record of ingests/updates
  overview.md               # synthesis of key themes across the engagement
  sources/
    client-brief.md         # summary page for raw/client-brief.md
  entities/
    dana-okafor.md
    meridian-markets.md
  concepts/
    prepared-foods-positioning.md
    loyalty-program.md
    store-expansion-strategy.md
  analyses/
    open-questions.md       # living list of open questions for Dana
    interview-prep.md       # synthesized interview question list/agenda
```

This is the starting shape given today's one source (`client-brief.md`).
Additional `entities/`, `concepts/`, and `analyses/` pages are added as
sources are ingested — the categories are fixed, the pages within them are
not.

## Page conventions

- **File naming**: kebab-case, `.md` extension, one page per distinct
  entity/concept/source/analysis (matches existing repo convention, e.g.
  `data-handling-checklist.md`).
- **Front matter**: none required. Keep pages plain markdown, consistent
  with the two existing docs in this repo.
- **Linking**: wiki-style `[[page-name]]` links between wiki pages wherever
  one page references another (e.g. `concepts/loyalty-program.md`
  references `[[entities/meridian-markets]]`). A link to something that
  doesn't have a page yet is fine — it marks a page worth writing later, not
  an error.
- **Citations to raw sources**: every wiki page must cite which raw
  source(s) it's derived from, e.g. `Source: [[sources/client-brief]]` near
  the top of the page.

### `sources/*.md`

One page per document in `raw/`. Contains: a short summary, key facts
extracted, and links out to the entity/concept pages it informed. Does not
duplicate the full source text.

### `entities/*.md`

People, organizations, and named things (Dana Okafor, Meridian Markets, the
Pasadena site once it's discussed). Contains: what's known about the
entity, sourced facts, and links to relevant concepts/analyses.

### `concepts/*.md`

Recurring ideas, methods, or themes (prepared-foods positioning, the
loyalty program, store-expansion strategy). Contains: definition/context as
it applies to this engagement, sourced facts, open sub-questions.

### `analyses/*.md`

Synthesis, comparison, and investigation pages that draw on multiple
sources/entities/concepts. Two specific pages required at this stage:

- **`analyses/open-questions.md`** — the single living list of open
  questions for Dana. This replaces the "Open questions for Dana" section
  currently embedded in `docs/data-handling-checklist.md`; that section is
  reduced to a one-line pointer linking here, so there is one source of
  truth instead of two lists that can drift apart.
- **`analyses/interview-prep.md`** — the ready-to-use interview question
  list/agenda for the Dana Okafor stakeholder interview, synthesized from
  the entity and concept pages. This is the direct deliverable the wiki
  exists to produce right now.

## Data-safety rule (binding on every wiki page)

Per the client brief's non-negotiable terms (also captured in
`docs/data-handling-checklist.md`): customer records, employee data,
loyalty program data, and labor schedules — including any excerpts — must
never enter an AI tool. Since the wiki itself is LLM-authored, this
translates into a hard rule enforced by `CLAUDE.md`:

- Wiki pages may describe a Restricted source only **structurally** —
  e.g. row counts, date ranges, field/column names present, high-level
  shape — and must never quote, paraphrase with specifics, or reproduce
  actual values from Restricted data.
- Sales totals by store/week and store attributes (square footage, opening
  date, lease terms) are the only data types the brief authorizes as Open;
  everything else defaults to Restricted, matching the classification
  already established in `docs/data-handling-checklist.md`.
- If it's unclear whether something is Restricted, the ingest workflow
  treats it as Restricted and flags it in `analyses/open-questions.md`
  rather than guessing.
- This rule applies retroactively to every future ingest, not just the
  current brief-only content (which is entirely Open/public-facing
  business narrative and has nothing Restricted in it today).

## Ingest workflow

Defined in `CLAUDE.md` for future sessions to follow:

1. Read the new raw source in `raw/`.
2. Apply the data-safety rule above to classify what can be captured.
3. Create or update the relevant `sources/`, `entities/`, `concepts/`, and
   `analyses/` pages, adding `[[wikilink]]` citations both ways.
4. Update `wiki/index.md` with any new/changed pages and their one-line
   summaries.
5. Append a dated entry to `wiki/log.md` describing what was ingested and
   what changed.

A source in `raw/` with no corresponding `sources/*.md` page is "pending
ingest" — this is a computed state (check `raw/` against `wiki/sources/`),
not something tracked separately, so it can't drift out of sync.

## Initial content (this pass)

Given the only current source is `raw/client-brief.md`, the first ingest
produces:

- `wiki/index.md`, `wiki/log.md`, `wiki/overview.md`
- `wiki/sources/client-brief.md`
- `wiki/entities/dana-okafor.md`, `wiki/entities/meridian-markets.md`
- `wiki/concepts/prepared-foods-positioning.md`,
  `wiki/concepts/loyalty-program.md`,
  `wiki/concepts/store-expansion-strategy.md`
- `wiki/analyses/open-questions.md` (seeded from the existing "Open
  questions for Dana" list in `docs/data-handling-checklist.md`)
- `wiki/analyses/interview-prep.md`
- `CLAUDE.md` at repo root
- An edit to `docs/data-handling-checklist.md` replacing its embedded
  open-questions list with a link to `wiki/analyses/open-questions.md`

## Out of scope

- Any tooling/automation to run the ingest workflow (this is a manual,
  LLM-assisted process for now, not a script).
- The NDA, POS extract, loyalty/labor data, or any future interview
  transcript — these are ingested in later passes once they exist.
- The dashboard/data-analysis deliverable itself.
