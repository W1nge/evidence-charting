# Evidence Charting

中文 | [English](README.md)

Evidence Charting 是一个 Codex skill，用来把有证据来源的调研结果整理成可审计的 Excel 图表工作簿。

它适合做模型跑分、竞品对比、产品价格、功能矩阵、公开数据调研等工作。核心目标不是只做漂亮图表，而是让每个图表都能追溯到证据、来源和缺失值规则。

## 这个 Skill 做什么

- 在制图前先寻找和评估证据。
- 保留来源链接、证据备注、冲突说明和 caveat。
- 缺失值保持空白，不把没找到的数据当作 0。
- 每个实体、模型或产品固定一个主题色，并在所有图和表格中保持一致。
- 生成带原始数据、证据备注、缺失值审计、颜色标准、图表分页的 Excel 工作簿。
- 默认把图表渲染成图片嵌入 Excel，避免原生 Excel 图表出现 `Series 1`、标签丢失、颜色错乱等问题。

## 安装

把仓库克隆到 Codex skills 目录：

```powershell
git clone https://github.com/W1nge/evidence-charting.git "$env:USERPROFILE\.codex\skills\evidence-charting"
```

如果已经安装，更新方式：

```powershell
cd "$env:USERPROFILE\.codex\skills\evidence-charting"
git pull
```

如果 Codex 没有马上识别到新 skill，重启 Codex 或开启一个新任务。

## 在 Codex 中使用

示例提示词：

```text
Use $evidence-charting to research the latest AI model benchmark scores and create an Excel workbook with source-backed bar charts.
```

```text
用 evidence-charting 调研几个竞品的价格、功能和公开数据，做成带来源和缺失值审计的 Excel 柱状图报告。
```

这个 skill 会按以下流程处理调研任务：

1. 发现证据并评估来源质量
2. 建立 evidence ledger
3. 结构化调研数据
4. 生成 Excel 工作簿和柱状图
5. 校验缺失值、来源、颜色一致性和图表可读性

## 直接运行脚本

也可以不通过 Codex，直接运行生成脚本：

```powershell
python .\scripts\make_evidence_charting_workbook.py --input .\data.json --output .\report.xlsx
```

运行自测：

```powershell
python .\scripts\make_evidence_charting_workbook.py --self-test --output "$env:TEMP\evidence-charting-self-test.xlsx"
```

Python 依赖：

- `openpyxl`
- `matplotlib`
- `numpy`

## 输入 JSON 示例

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

完整字段说明见 [references/data_schema.md](references/data_schema.md)。

## 输出工作簿结构

默认生成的 Excel 包含：

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

## 证据规则

尽量使用一手或可复现来源：

- 官方产品页
- 官方文档
- benchmark 原始表格
- 论文或数据集
- 公司公告、财报、监管文件
- 价格页、rate card
- 标准组织或监管机构页面

搜索结果摘要、论坛、社交媒体和转述文章只能作为线索。除非用户明确接受这类证据质量，否则不要把它们当成最终证据。

证据采集流程见 [references/evidence_finding.md](references/evidence_finding.md)。

## 校验

校验 skill 结构：

```powershell
$env:PYTHONUTF8 = "1"
python "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" "$env:USERPROFILE\.codex\skills\evidence-charting"
```

校验工作簿生成：

```powershell
python .\scripts\make_evidence_charting_workbook.py --self-test --output "$env:TEMP\evidence-charting-self-test.xlsx"
```

自测应满足：

- 工作簿可以打开
- 缺失值保持空白
- 0 分数量不会因为缺失值而被抬高
- 同一实体在图表和表格中颜色一致
- 来源页和证据备注字段存在

## 仓库结构

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

## 维护说明

更新这个 skill 时：

1. 保持 `SKILL.md` 简洁，把细节放到 `references/`。
2. 不要把生成出来的 Excel 工作簿提交到 git。
3. 运行 `quick_validate.py`。
4. 运行生成脚本自测。
5. 如果行为发生变化，把说明文件和脚本改动一起提交。
