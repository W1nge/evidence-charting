# Evidence Charting

[中文](README.zh.md) | English

Evidence Charting is a Codex skill for turning source-backed research into audited Excel workbooks with clear bar charts.

It is designed for work such as benchmark reports, model/product comparisons, competitor research, pricing comparisons, and other research tasks where the final chart should be traceable back to evidence.

## What This Skill Does

- Finds and qualifies evidence before charting.
- Preserves source URLs, caveats, and evidence notes.
- Keeps missing values blank instead of treating them as zero.
- Assigns one fixed theme color per entity and reuses it everywhere.
- Generates polished Excel workbooks with raw data, evidence notes, missing-value audit, color standards, and chart pages.
- Uses rendered chart images inside Excel to avoid unstable native chart labels such as `Series 1`.

## Install

Clone this repository into your Codex skills directory:

```powershell
git clone https://github.com/W1nge/evidence-charting.git "$env:USERPROFILE\.codex\skills\evidence-charting"
```

If it is already installed, update it with:

```powershell
cd "$env:USERPROFILE\.codex\skills\evidence-charting"
git pull
```

Restart Codex or start a new task if the skill list does not refresh immediately.

## Use In Codex

Example prompts:

```text
Use $evidence-charting to research the latest AI model benchmark scores and create an Excel workbook with source-backed bar charts.
```

```text
用 evidence-charting 调研几个竞品的价格、功能和公开数据，做成带来源和缺失值审计的 Excel 柱状图报告。
```

The skill routes research work through:

1. evidence discovery and source qualification
2. evidence ledger construction
3. structured data normalization
4. workbook/chart generation
5. missing-value and color-consistency validation

## Direct Script Usage

The workbook generator can also be run directly:

```powershell
python .\scripts\make_evidence_charting_workbook.py --input .\data.json --output .\report.xlsx
```

Run the self-test:

```powershell
python .\scripts\make_evidence_charting_workbook.py --self-test --output "$env:TEMP\evidence-charting-self-test.xlsx"
```

Python dependencies:

- `openpyxl`
- `matplotlib`
- `numpy`

## Input JSON Shape

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
        "gpt56_sol": 64.6
      },
      "sources": [
        "https://example.com/source"
      ],
      "evidence_notes": "Optional caveat or extraction note."
    }
  ],
  "comparison": {
    "primary_group": "OpenAI GPT-5.6",
    "baseline_entity": "gpt55",
    "external_groups": ["Anthropic Claude", "Google Gemini"]
  }
}
```

See [references/data_schema.md](references/data_schema.md) for the full schema notes.

## Workbook Output

The default workbook contains:

- `00_阅读说明`
- `01_总览`
- `02_所有柱状图`
- `03_差距对比图`
- `04_按领域`
- `05_颜色标准`
- `06_缺失值审计`
- `07_差距摘要`
- `08_原始数据`
- `09_来源`

## Evidence Rules

Use primary or reproducible sources whenever possible:

- official product pages
- documentation
- benchmark tables
- papers or datasets
- filings
- pricing pages
- standards or regulator sources

Search snippets, forum posts, and social posts should be treated as discovery leads, not evidence, unless the user explicitly accepts that source quality.

See [references/evidence_finding.md](references/evidence_finding.md) for the evidence workflow.

## Validation

Validate the skill structure:

```powershell
$env:PYTHONUTF8 = "1"
python "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" "$env:USERPROFILE\.codex\skills\evidence-charting"
```

Validate workbook generation:

```powershell
python .\scripts\make_evidence_charting_workbook.py --self-test --output "$env:TEMP\evidence-charting-self-test.xlsx"
```

Expected self-test behavior:

- workbook opens
- no Excel repair prompt from generated table XML
- missing values remain blank
- zero count is not inflated by missing values
- entity colors stay consistent across charts and tables
- source and evidence-note sheets are present

## Repository Layout

```text
evidence-charting/
  SKILL.md
  agents/openai.yaml
  references/
    data_schema.md
    evidence_finding.md
    workbook_contract.md
  scripts/
    make_evidence_charting_workbook.py
```

## Maintenance Notes

When updating the skill:

1. Keep `SKILL.md` concise and route longer details into `references/`.
2. Keep generated workbooks out of git.
3. Run `quick_validate.py`.
4. Run the generator self-test.
5. Check generated `.xlsx` packages for unexpected `xl/tables/table*.xml` parts when workbook structure changes.
6. Commit both instruction and script changes together when behavior changes.
