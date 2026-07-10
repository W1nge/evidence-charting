# Data Schema

Use this JSON shape with `scripts/make_evidence_charting_workbook.py`.

```json
{
  "title": "Benchmark comparison report",
  "generated_at": "2026-07-10",
  "source_note": "Primary sources retrieved on 2026-07-10.",
  "entities": [
    {
      "id": "gpt56_sol",
      "label": "GPT-5.6 Sol",
      "group": "OpenAI GPT-5.6",
      "color": "#0B5CAD"
    }
  ],
  "metrics": [
    {
      "category": "Coding",
      "name": "SWE-Bench Pro",
      "unit": "%",
      "higher_is_better": true,
      "scores": {
        "gpt56_sol": 64.6,
        "competitor": null
      },
      "sources": [
        {
          "url": "https://example.com/source",
          "publisher": "Example Lab",
          "retrieved_at": "2026-07-10",
          "source_type": "official benchmark",
          "confidence": "high",
          "date_scope": "v1.2 results",
          "caveat": "Vendor-reported result",
          "extraction_method": "HTML table"
        }
      ],
      "evidence_notes": "Optional caveat, extraction note, or conflict note."
    }
  ],
  "comparison": {
    "primary_group": "OpenAI GPT-5.6",
    "baseline_entity": "gpt55",
    "external_groups": ["Anthropic Claude", "Google Gemini"]
  }
}
```

## Field Notes

- `entities[].id`: stable ASCII key used in scores.
- `entities[].label`: display name shown in charts and tables.
- `entities[].group`: used for grouped comparisons.
- `entities[].color`: fixed theme color. If omitted, the script assigns a palette color, but explicit colors are better.
- `metrics[].scores`: omit missing values or set them to `null`; do not use `0` for missing.
- `metrics[].sources`: source objects are preferred. A URL string remains accepted for backward compatibility.
- Source object fields: `url` (or a clear `user-provided data` note), `publisher`, `retrieved_at`, `source_type`, `confidence`, `date_scope`, `caveat`, and `extraction_method`.
- `metrics[].evidence_notes`: optional caveats such as conflicting sources, rollout wording, secondary-only source, or extraction from image/PDF.
- `metrics[].higher_is_better`: defaults to `true`.
- `comparison.primary_group`: group whose best entity is compared against baseline/external entities.
- `comparison.baseline_entity`: optional entity id for baseline comparisons.
- `comparison.external_groups`: optional list of groups considered external competitors.

## Minimal Viable Dataset

Require at least:

- two entities
- one metric
- at least one metric with two numeric scores for a real comparison

A metric with only one numeric score is allowed, but it is coverage-only and must not be presented as a comparison.

For a useful report, prefer at least three entities and five metrics.
