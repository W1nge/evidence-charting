# Workbook Contract

Use this contract for multi-chart research workbooks.

## Visual Rules

- Use one theme color per entity across every chart and table.
- Put a color-standard sheet before audit/raw-data sheets.
- Prefer styled ranges with autofilter over native Excel Table objects unless editability requires tables and the generated file is verified in desktop Excel.
- Use single-column vertical chart layout by default.
- Label every bar with the numeric value.
- Put entity names on the chart axis or inside labels. Never rely on `Series 1`, `Series 2`, or positional legends.
- Keep each chart to one metric or one compatible unit group.
- For many metrics, create one chart per metric and one composite chart per category.
- Size text columns by rendered character width, treating CJK/full-width characters as wider than Latin text.
- Wrap long text and estimate row height from the final column width. Cap row height so one record cannot dominate a sheet.
- For exceptionally long notes, show a readable excerpt in the cell and preserve the complete text in a cell comment.

## Data Rules

- Keep missing values as blank/null.
- Do not impute missing values unless the user explicitly asks.
- Add a missing-value audit with:
  - total entity-score cells
  - numeric cells
  - blank cells
  - zero-valued cells
  - per-entity coverage
- Track `higher_is_better` per metric.
- Use source-backed rows. Each metric row should have at least one source URL or note.
- Preserve evidence quality notes when data came from secondary sources, conflicting sources, images/PDFs, or rollout language.

## Comparison Rules

- Compare scores only where both sides have data.
- Prefer actual score bars over percent-difference bars.
- If a "best target group vs baseline/external" view is useful, show the actual participating entities as bars using their theme colors.
- Keep percent deltas in the summary table, not as the primary visual, unless the user explicitly requests percent-difference charts.

## Validation Checklist

- [ ] Workbook opens.
- [ ] The `.xlsx` package has no unexpected `xl/tables/table*.xml` parts unless native tables were explicitly required and tested.
- [ ] Expected sheets exist.
- [ ] Missing-value audit arithmetic is correct.
- [ ] No missing score was converted to `0`.
- [ ] Entity colors match the color-standard sheet.
- [ ] At least one chart image was visually inspected.
- [ ] Source sheet links back to the research evidence.
- [ ] Source gaps and conflicts are visible, not hidden behind polished charts.
- [ ] Long CJK/English notes wrap cleanly, row heights are readable, and exceptionally long text remains available in comments.
