# DA02 Consumer Credit Data (Federal Reserve) — Project Plan

## Problem Statement
> *"How has U.S. consumer credit evolved since the COVID shock, and which holder types and credit segments pose the highest risk of delinquency stress going into 2025–2026?"*

---

## Sub-Questions
*Size → Rank → Explain → Compare → Recommend framework*

| # | Type | Question |
|---|------|----------|
| 1 | Size | What is total consumer credit outstanding by holder type and credit category, and how has it changed from 2020 to 2025? |
| 2 | Rank | Which holder types have experienced the highest delinquency growth rates since the post-COVID rate hike cycle began? |
| 3 | Explain | What drives variance in consumer credit growth across holder types — is it credit type mix, rate sensitivity, or borrower segment? |
| 4 | Compare | How does revolving credit behavior differ from nonrevolving credit across the COVID shock, recovery, and rate hike eras? |
| 5 | Recommend | Which 3 credit segments warrant the closest monitoring based on delinquency trajectory and net flow stress signals? |

*Audience: General analyst — recommendations should explain what to watch, why it matters, and what the data signals, without assuming a specific portfolio or risk management action.*

---

## Deliverables

| File | Purpose | Format Reference |
|------|---------|-----------------|
| `DA02_Analysis.ipynb` | Data wrangling, enrichment, and metric computation (Stages 1–5) | `/analyze` skill |
| `DA02_Final-Report.ipynb` | Question-driven data story and recommendations (Q1–Q5) | `/analyze` + `/validate-data` |
| `credit_analysis_clean.csv` | Clean output from analysis notebook, input for final report | Already exists at `data/` |
| `DA02_Report.html` | McKinsey-style interactive HTML report | `/design` skill |

---

## Data Source
- **Source name:** Federal Reserve G.19 Consumer Credit Report
- **Access:** [https://www.federalreserve.gov/releases/g19/](https://www.federalreserve.gov/releases/g19/)
- **Table used:** Consumer Credit Primary Outstanding (Levels) — Not Seasonally Adjusted
- **Files:**

| File | Description |
|------|-------------|
| `FRB_G19_(2).csv` | Raw source — wide format, rows = series, columns = months (2020-01 → 2025-01) |
| `credit_long_raw.csv` | Unpivoted long format — one row per series × month |
| `credit_analysis_clean.csv` | Cleaned data with growth metrics and era labels |
| `credit_total_monthly.csv` | Aggregated total outstanding by month |
| `credit_type_pivot.csv` | Pivoted by credit type (Total / Revolving / Nonrevolving) |
| `credit_holder_pivot.csv` | Pivoted by holder type |
| `credit_flow_pivot.csv` | Monthly net flow by holder |
| `credit_yoy_pivot.csv` | Year-over-year growth rates by holder |

### Key Column Mappings

| Column | Description |
|--------|-------------|
| `Series Name` | Unique series code (e.g., `DTCNLHD_N.M`) |
| `date` | Monthly date (YYYY-MM-DD) |
| `holder` | Credit holder type (see Lookup Codes below) |
| `credit_type` | Total / Revolving / Nonrevolving |
| `ownership_type` | Owned vs. Securitized |
| `measure` | Level (outstanding balance) or Flow (monthly net change) |
| `value_millions` | Credit balance or flow in USD millions |
| `yoy_growth_pct` | Year-over-year % change in value |
| `mom_growth_pct` | Month-over-month % change in value |
| `rolling_3m_avg` | 3-month rolling average of value |
| `era` | Economic era label (see Lookup Codes below) |

### Lookup / Reference Codes

| Code | Meaning |
|------|---------|
| `Depository Institutions` | Banks and savings institutions |
| `Finance Companies` | Non-bank lenders (auto, consumer finance) |
| `Credit Unions` | Member-owned cooperative lenders |
| `Federal Government` | Primarily student loans (Dept. of Education) |
| `Nonprofit and Educational Institutions` | University and nonprofit lenders |
| `Owned` | Credit held on the institution's own balance sheet |
| `Securitized` | Credit packaged into asset-backed securities |
| `Level` | Outstanding balance at end of period |
| `Flow` | Net monthly change (new credit minus repayments/charge-offs) |
| `Pre-COVID` | Jan 2020 – Feb 2020 |
| `COVID Shock` | Mar 2020 – Dec 2020 |
| `Recovery` | Jan 2021 – Feb 2022 |
| `Rate Hike Cycle` | Mar 2022 – Dec 2023 |
| `Post-Hike` | Jan 2024 – Jan 2025 |

**Delinquency strategy:** Net flow deterioration will be used as the delinquency stress proxy. No supplemental data source required — all analysis stays within the G.19 dataset.

---

## Notebook 1 — `DA02_Analysis.ipynb`
*5-Stage wrangling-first approach · run via `/analyze`*

### Stage 1: Import & Load Data
- Import libraries: `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly`
- Load `FRB_G19_(2).csv` and preview raw shape, columns, dtypes, and null counts
- Confirm date range: Jan 2020 – Jan 2025 (61 months)
- Document all series identifiers and map to holder / credit type / measure taxonomy

### Stage 2: Data Wrangling
- Unpivot wide format → long format (already done → `credit_long_raw.csv`)
- Parse `date` column to datetime; set as index
- Cast `value_millions` to float; handle zero-fill vs. true nulls
- Drop series with all-zero values (e.g., finance company securitized, which is fully zero in this period)
- Tag each row with `holder`, `credit_type`, `ownership_type`, and `measure` from series description

### Stage 3: Merge & Enrich
- Join on `Series Name` to attach holder taxonomy labels
- Add `era` column based on date ranges defined in Lookup Codes
- No supplemental data merge required — delinquency stress inferred from net flow deterioration

### Stage 4: Compute Key Metrics

| Metric | Formula |
|--------|---------|
| `yoy_growth_pct` | `(value_t - value_t-12) / abs(value_t-12) * 100` |
| `mom_growth_pct` | `(value_t - value_t-1) / abs(value_t-1) * 100` |
| `rolling_3m_avg` | `value.rolling(3).mean()` |
| `net_flow_millions` | Use `measure == 'Flow'` series directly |
| `holder_share_pct` | `holder_value / total_value * 100` per month |
| `revolving_share_pct` | `revolving_value / total_value * 100` per month |
| `era` | Date-based label per Lookup Codes |

### Stage 5: Profile & Validate
1. **Value distributions** — histograms of `value_millions` by holder and credit type
2. **Segment distribution** — holder share breakdown by era
3. **Cross-sectional comparison** — YoY growth by holder across all periods
4. **Outlier detection** — flag monthly flows outside ±2 std dev; review COVID-era spikes

Additional checks:
- Confirm holder subtotals sum to total consumer credit each month
- Validate no negative level values (flow can be negative)
- Confirm `credit_analysis_clean.csv` YoY calculations match manual spot-checks
- Export clean dataset → `data/credit_analysis_clean.csv`

---

## Notebook 2 — `DA02_Final-Report.ipynb`
*Question-driven data story · run via `/analyze` then `/validate-data`*

### Opening
- Title: *"U.S. Consumer Credit: Stress Signals in a Post-Pandemic Economy"*
- 2–3 sentence context paragraph: G.19 dataset scope, 2020–2025 coverage, five holder types
- Load `data/credit_analysis_clean.csv`

---

### Question 1 — Size
**"What is total consumer credit outstanding by holder type, and how has it changed from 2020 to 2025?"**
- **Approach:** Aggregate `value_millions` by `holder` and `date` where `measure == 'Level'`; compute absolute and percentage change from Jan 2020 baseline
- **Visualization:** Stacked area chart — time on x-axis, credit outstanding ($B) on y-axis, one band per holder; annotate era boundaries
- **What did we learn?** Scale of credit expansion post-COVID, which holders grew fastest, total credit in Jan 2025 vs. Jan 2020

---

### Question 2 — Rank
**"Which holder types have experienced the highest delinquency growth rates since the rate hike cycle began?"**
- **Approach:** Rank holders by YoY growth in net flow deterioration (negative flow acceleration) from Mar 2022 onward, used as the delinquency stress proxy
- **Visualization:** Ranked horizontal bar chart or lollipop chart — one bar per holder, sorted by stress metric
- **What did we learn?** Which holders are most exposed; whether depository institutions or federal government loans show more stress

---

### Question 3 — Explain
**"What drives variance in consumer credit growth across holder types?"**
- **Approach:** Correlation analysis between `yoy_growth_pct` across holders; test whether revolving share predicts growth volatility; era-segmented comparison
- **Visualization:** Scatter matrix or heatmap of YoY growth correlations across holders; annotate COVID shock and rate hike inflection points
- **What did we learn?** Whether holder growth rates move together or diverge; which holders are rate-sensitive vs. structurally driven

---

### Question 4 — Compare
**"How does revolving credit behavior differ from nonrevolving across the COVID shock, recovery, and rate hike eras?"**
- **Approach:** Compute `revolving_share_pct` and `nonrevolving_share_pct` by era; compare MoM flow patterns for each type across all five eras
- **Visualization:** Dual-axis line chart (revolving vs. nonrevolving outstanding) with era shading; secondary chart showing net flow by type
- **What did we learn?** Whether revolving credit is signaling consumer stress earlier than nonrevolving; how the credit mix shifted post-COVID

---

### Question 5 — Recommend
**"Which 3 credit segments warrant the closest monitoring based on delinquency trajectory and net flow stress signals?"**
- **Approach:** Score each holder × credit type combination on: (1) rate of net flow deterioration, (2) YoY growth deceleration, (3) share of total credit; rank and select top 3
- **Top 3 Opportunities:** *(to be completed after analysis)*
  1. **Federal Government** — Student loan portfolio: largest flow deterioration (-$2,853M/mo), 43% negative months. Watch for post-repayment resumption default cascade.
  2. **Finance Companies** — Auto/consumer loans: fastest growth-to-deceleration (30% → 1.4% YoY). Subprime auto book seasoning under higher rates.
  3. **Depository Institutions (Revolving)** — Credit card: revolving share at cycle high (25.9%), deceleration from 13.5% → 5.8% YoY puts us in the 6–12 month charge-off lag window.
- **Visualization:** Opportunity matrix — stress score vs. credit size; annotate top 3 with callout boxes
- **What did we learn?** Prioritized watchlist with plain-language explanation of what each segment's data signals and why a general analyst should keep watching it

---

## Workflow

```
Source Files
  (FRB_G19_(2).csv)
           │
           ▼
DA02_Analysis.ipynb          ← /analyze
  Stage 1: Import & Load
  Stage 2: Wrangle
  Stage 3: Merge & Enrich    ← ⚠️ Delinquency source TBD
  Stage 4: Compute Metrics
  Stage 5: Profile & Validate
           │
           ▼
    data/credit_analysis_clean.csv
           │
           ▼
DA02_Final-Report.ipynb      ← /analyze → /validate-data
  Q1: Size → Q2: Rank → Q3: Explain
  Q4: Compare → Q5: Recommend
           │
           ▼
    DA02_Report.html          ← /design
```

---

## Status Tracker

| Task | Status |
|------|--------|
| Define problem statement and sub-questions | ✅ Done |
| Identify and access data source | ✅ Done |
| Document column mappings and lookup codes | ✅ Done |
| Create `DA02_Project-Plan.md` | ✅ Done |
| Clarify report audience for Q5 Recommend | ✅ Done — general analyst audience |
| Confirm delinquency data strategy (proxy vs. supplemental source) | ✅ Done — net flow deterioration as stress proxy |
| Build Analysis notebook — Stages 1–2 (wrangle & clean) | ✅ Done — `DA02_Analysis.ipynb` |
| Build Analysis notebook — Stage 3 (merge & enrich) | ✅ Done — era remapped to 5-bucket plan |
| Build Analysis notebook — Stages 4–5 (metrics & profiling) | ✅ Done — all 5 validation checks pass |
| Run `/validate-data` QA check | ⬜ Pending |
| Build Final Report notebook (Q1–Q5) | ✅ Done — `DA02_Final-Report.ipynb` (Q1–Q5 + 8 charts) |
| Run `/design` to generate HTML report | ⬜ Pending |
| Prep repo for GitHub | ⬜ Pending |
