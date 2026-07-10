---
name: evidence-charting
description: >
  Load when the user asks for evidence-backed research charts, 柱状图调研,
  benchmark research, score comparisons, competitor/model/product comparisons,
  or an Excel report with sourced data, evidence audit, bar charts,
  missing-value audit, fixed entity colors, and polished chart pages. Use for
  requests like "做一个调研 Excel 柱状图", "跑分对比", "竞品对比图",
  "research this and make charts", or "source-backed comparison workbook". Do
  not use for simple one-off chart edits when the user only needs a quick
  in-chat answer.
---

# Evidence Charting

Create source-backed research workbooks with clear bar charts. The default output is an Excel file with raw data, missing-value audit, fixed entity colors, chart pages, and source links.

## Common Tasks

| Task | Required reads |
|---|---|
| Research + Excel chart report | `references/evidence_finding.md`, `references/workbook_contract.md`, `references/data_schema.md`; then run/adapt `scripts/make_evidence_charting_workbook.py` |
| User provides structured data and only wants the workbook | `references/data_schema.md`; run `scripts/make_evidence_charting_workbook.py` |
| User asks "find sources/evidence first" | `references/evidence_finding.md`; build an evidence ledger before charting |
| Fix or review an existing chart workbook | `references/workbook_contract.md`; inspect workbook sheets/charts/images before editing |
| Other visualization request | Read this file; use the contract when the output is a multi-chart research workbook |

## Workflow

1. Define the comparison scope.
   - Identify entities, metrics, units, categories, and whether higher or lower is better.
   - Decide whether the task needs current public research, user-provided data, internal/local files, or a mixture.
   - If any data point is not already provided by the user, read `references/evidence_finding.md` and build an evidence ledger before charting.

2. Find and qualify evidence before extracting numbers.
   - Prefer primary/official sources for product facts, benchmark tables, prices, filings, laws, docs, and current claims.
   - Use secondary sources only to discover leads or when primary sources are unavailable; label them as secondary.
   - Record source URL, publisher, retrieval date, source type, claim/metric covered, and any caveat.
   - If sources conflict, keep the conflict visible instead of averaging or silently choosing.

3. Normalize the data before making charts.
   - Keep missing values blank. Never convert an unpublished or unfound score to `0` unless the user explicitly says missing means zero.
   - Keep one raw table where each row is one metric and each entity is one score column.
   - Add category, unit, direction, source, retrieval date, and evidence quality.

4. Lock the visual contract before batch generation.
   - Assign one theme color per entity/model/product and reuse it in every chart and table label.
   - Use single-column vertical chart layout by default. Avoid side-by-side image placement in Excel.
   - Do not mix incompatible units in one chart.
   - Prefer rendered image charts embedded into Excel for stable labels and colors. Use native Excel charts only when editability is more important and verify labels in Excel.

5. Generate the workbook.
   - Convert researched data into the JSON schema in `references/data_schema.md`.
   - Run:

```bash
python <skill-dir>/scripts/make_evidence_charting_workbook.py --input data.json --output report.xlsx
```

6. Validate before responding.
   - Open/read the workbook and verify expected sheets exist.
   - Check missing-value audit: numeric + blank = entity cells, and zero count is not inflated by missing values.
   - Inspect at least one chart image or screenshot when visual quality matters.
   - Confirm each entity uses the same theme color across chart pages and table headers.
   - Confirm source URLs are present and clickable where possible.
   - Confirm every charted metric has a source note or source URL.

## Default Workbook Structure

Use this order unless the user asks otherwise:

1. `00_阅读说明`
2. `01_总览`
3. `02_所有柱状图`
4. `03_差距对比图` when a primary group/baseline comparison exists
5. `04_按领域`
6. `05_颜色标准`
7. `06_缺失值审计`
8. `07_差距摘要`
9. `08_原始数据`
10. `09_来源`

## Known Gotchas

- Search results are discovery, not evidence. Open the underlying source and cite the original page, filing, paper, dataset, or documentation.
- Excel native charts may show `Series 1`, lose model labels, or render differently across environments. Prefer image charts for deliverables.
- Relative differences can explode when the baseline is very small. Prefer raw side-by-side score bars for visual comparison; keep percent deltas in tables.
- Cross-metric averages are often misleading when entity coverage differs. Show coverage first; only compute synthetic scores when the user explicitly asks and the method is documented.
- Missing benchmark scores are not zero. Audit this explicitly.
- Colors are identity, not decoration: the same entity must keep the same theme color everywhere.
- If evidence is weak, make the workbook say so. Do not let polished charts hide uncertain sourcing.

## Output Summary

In the final response, link the Excel file and state:

- data source level used, such as official/primary sources or user-provided data
- evidence gaps or source conflicts
- missing-value policy
- number of metrics, entities, and embedded chart images
- any unresolved source gaps or assumptions
