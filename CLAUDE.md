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
