# [Project ID] [Project Name] — Project Plan

## Problem Statement
> *"[Write your central analytical question here. Frame it as a business problem, not a technical one.]"*

---

## Sub-Questions
*Use the Size → Rank → Explain → Compare → Recommend framework to break the central question into 5 answerable sub-questions.*

| # | Type | Question |
|---|------|----------|
| 1 | Size | *What is [key metric] by [unit of analysis]?* |
| 2 | Rank | *Which [units] are highest / lowest performing?* |
| 3 | Explain | *What drives variance in [key metric]?* |
| 4 | Compare | *How does [variable] impact [outcome]?* |
| 5 | Recommend | *Which [N] opportunities have the highest ROI?* |

---

## Deliverables

| File | Purpose | Format Reference |
|------|---------|-----------------|
| `[ProjectID]_Analysis.ipynb` | Data wrangling, enrichment, and metric computation | Example01_Data-Wrangling-Analysis |
| `[ProjectID]_Final-Report.ipynb` | Question-driven data story and recommendations | Example02_Data-Story |
| `[project]_clean.csv` | Clean output from analysis notebook, input for final report | — |

---

## Data Source
- **Source name:** *[Name of dataset / provider]*
- **Access:** *[URL, API, file download, Google Sheets, etc.]*
- **Files:** *[List source files and what each contains]*

### Key Column Mappings
*Document the columns you'll rely on. Add or remove rows as needed.*

| Column | Description |
|--------|-------------|
| `[ID_COL]` | Unique record identifier |
| `[DATE_COL]` | Date or time period |
| `[SEGMENT_COL]` | Grouping / segmentation variable |
| `[METRIC_COL_1]` | Primary metric |
| `[METRIC_COL_2]` | Secondary metric |

### Lookup / Reference Codes
*If your data uses coded values (e.g. category codes, status flags), document them here.*

| Code | Meaning |
|------|---------|
| `[CODE_1]` | *Description* |
| `[CODE_2]` | *Description* |

---

## Notebook 1 — `[ProjectID]_Analysis.ipynb`
*5-Stage wrangling-first approach*

### Stage 1: Import & Load Data
- Import libraries: `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly`
- Load source files and preview raw shape, columns, dtypes, and null counts
- Define and document key column mappings

### Stage 2: Data Wrangling
- Filter rows to relevant records (describe your filter logic here)
- Reshape data if needed (e.g. pivot long → wide)
- Cast column types (dates, IDs, numeric amounts)
- Remove invalid records (nulls, zeros, duplicates — specify rules)

### Stage 3: Merge & Enrich
- Join source files on `[join keys]`
- Bring in supplemental attributes: *[list enrichment fields]*
- Align date/time fields for consistent joins

### Stage 4: Compute Key Metrics
*Define every derived column you plan to use in the Final Report.*

| Metric | Formula |
|--------|---------|
| `[metric_1]` | `[formula]` |
| `[metric_2]` | `[formula]` |
| `[metric_3]` | `[formula]` |
| `[segment_label]` | `[segmentation logic]` |

### Stage 5: Profile & Validate
1. **Value distributions** — histograms of key numeric columns
2. **Segment distribution** — breakdown by primary grouping variable
3. **Cross-sectional comparison** — box plots or bar charts by region / category
4. **Outlier detection** — flag records outside `[threshold]`, isolate for review

Additional checks:
- Segment records into performance tiers: **High / Medium / Low**
- Sanity checks: subtotals reconcile, no negative values, null rate per column
- Export clean dataset → `data/processed/[project]_clean.csv`

---

## Notebook 2 — `[ProjectID]_Final-Report.ipynb`
*Question-driven data story*

### Opening
- Title: *"[Restate the central problem statement]"*
- 2–3 sentence context paragraph: dataset, scope, and what the analysis covers
- Load `data/processed/[project]_clean.csv`

---

### Question 1 — Size
**"[Restate Q1 from Sub-Questions table]"**
- **Approach:** *[Describe the calculation or aggregation]*
- **Visualization:** *[Chart type and axes]*
- **What did we learn?** *[What the chart should reveal]*

---

### Question 2 — Rank
**"[Restate Q2]"**
- **Approach:** *[Describe ranking methodology and any segmentation overlay]*
- **Visualization:** *[Chart type — e.g. ranked dot plot, lollipop, sorted bar]*
- **What did we learn?** *[Shared traits of top / bottom performers]*

---

### Question 3 — Explain
**"[Restate Q3]"**
- **Approach:** *[Describe regression, correlation, or residual analysis]*
- **Visualization:** *[Scatter plot, residuals plot, or feature importance chart]*
- **What did we learn?** *[Key drivers and unexplained variance]*

---

### Question 4 — Compare
**"[Restate Q4]"**
- **Approach:** *[Describe grouping logic and comparison method]*
- **Visualization:** *[Grouped bar, stacked area, or faceted chart]*
- **What did we learn?** *[How the variable shifts the outcome]*

---

### Question 5 — Recommend
**"[Restate Q5]"**
- **Approach:** *[Describe peer benchmarking and savings / ROI estimation method]*
- **Top [N] Opportunities:**
  1. *[Opportunity 1]*
  2. *[Opportunity 2]*
  3. *[Opportunity 3]*
- **Visualization:** *[Waterfall chart, prioritized table, or opportunity matrix]*
- **What did we learn?** *[Quantified ROI and prioritized action list]*

---

## Workflow

```
Source Files
  ([File 1], [File 2], [File 3])
           │
           ▼
[ProjectID]_Analysis.ipynb
  Stage 1: Import & Load
  Stage 2: Wrangle
  Stage 3: Merge & Enrich
  Stage 4: Compute Metrics
  Stage 5: Profile & Validate
           │
           ▼
    data/processed/[project]_clean.csv
           │
           ▼
[ProjectID]_Final-Report.ipynb
  Q1: Size → Q2: Rank → Q3: Explain
  Q4: Compare → Q5: Recommend
           │
           ▼
    [ProjectID]_Report.html
```

---

## Status Tracker

| Task | Status |
|------|--------|
| Define problem statement and sub-questions | ⬜ Pending |
| Identify and access data source | ⬜ Pending |
| Document column mappings and lookup codes | ⬜ Pending |
| Create `PROJECT_PLAN.md` | ⬜ Pending |
| Build Analysis notebook (Stages 1–5) | ⬜ Pending |
| Run `/validate-data` QA check | ⬜ Pending |
| Build Final Report notebook (Q1–Q5) | ⬜ Pending |
| Run `/design` to generate HTML report | ⬜ Pending |
| Prep repo for GitHub | ⬜ Pending |
