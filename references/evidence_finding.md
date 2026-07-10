# Evidence Finding

Use this when data must be researched before charting.

## Evidence Ladder

Prefer the highest available source tier:

1. Primary source: official product page, documentation, dataset, benchmark table, filing, law/regulation text, paper, standards body, vendor pricing page.
2. Reproducible source: public repo, benchmark harness, archived dataset, downloadable CSV/PDF, methodology page.
3. Credible secondary source: reputable media, analyst report, trusted aggregator, independent benchmark writeup.
4. Search snippet/social/forum: discovery only, not evidence unless the user explicitly accepts it.

Default rule: chart primary or reproducible data. Use secondary data only when primary data is unavailable, stale, inaccessible, or the task explicitly asks for market/media views.

## Search Procedure

1. Convert the user question into entities, metrics, date scope, and required source type.
2. Search narrowly for official or source-owning pages first.
   - Product/model/API facts: official docs, release pages, help center, model pages.
   - Benchmarks: original benchmark table, system card, paper, dataset, benchmark harness.
   - Pricing/availability: vendor pricing pages, plan pages, help center, rate cards.
   - Companies/finance: filings, investor relations, exchange/regulator pages.
   - Legal/policy: issuing authority or official gazette.
3. Open the actual source page and extract the relevant table/claim.
4. Cross-check high-impact values against a second source when feasible.
5. Record gaps immediately; do not postpone uncertainty until charting.

## Evidence Ledger

Keep a compact ledger while researching. Minimum fields:

| Field | Meaning |
|---|---|
| `metric` | Exact metric/claim supported |
| `entity` | Entity/model/product the data belongs to |
| `value` | Extracted value, or blank if unavailable |
| `unit` | %, USD, tokens, Elo, ms, etc. |
| `date_scope` | Publish date, benchmark version, price effective date, retrieval date |
| `source_url` | Direct URL to original source |
| `source_type` | primary, reproducible, secondary, discovery-only |
| `confidence` | high, medium, low |
| `caveat` | Missing values, conflicting values, methodology caveat, paywall, rollout note |

For Excel output, preserve this ledger in sources/raw-data notes even if the chart only displays values.

## Extraction Rules

- Preserve units exactly, then normalize only after recording the original.
- Do not average conflicting numbers unless the user asks for a derived statistic.
- Do not convert "not reported", "not tested", "not available", or missing cells to zero.
- If a metric's direction is ambiguous, stop and decide before charting.
- If a value comes from an image/PDF/table, record that extraction method in `caveat`.
- If a page has rollout language like "gradual", "preview", "beta", or "selected users", include that caveat.

## Conflict Handling

When sources disagree:

1. Prefer the primary source with the closest date and exact metric definition.
2. If both are primary but differ, keep both in the ledger and mark the charted value as chosen with caveat.
3. If an old source and a newer source conflict, use the newer source and record the older source as superseded.
4. If no defensible choice exists, leave the value blank and add a source gap.

## Minimum Evidence Before Charting

- Every charted metric must have at least one source URL or user-provided-data note.
- Every entity should have a coverage count, so missing data is visible.
- Any secondary-only metric should be labeled in notes.
- Any synthetic score or normalized ranking must state its formula and source coverage.
