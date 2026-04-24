# U.S. Consumer Credit Analysis — Federal Reserve G.19

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=matplotlib&logoColor=white)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-444876?style=for-the-badge&logo=seaborn&logoColor=white)](https://seaborn.pydata.org/)
[![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)](https://scipy.org/)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/)

## Overview

End-to-end analysis of U.S. consumer credit data from the Federal Reserve's G.19 statistical release (Not Seasonally Adjusted), covering January 2020 through January 2025. The project investigates how consumer credit evolved since the COVID shock and identifies which holder types and credit segments pose the highest risk of delinquency stress heading into 2025–2026.

**Central Finding:** Credit card (revolving) flows have been negative since mid-2023, signaling a 6–12 month lead time to charge-off peaks projected for Q2–Q3 2025. Credit Unions are currently the most stressed lender cohort, while Depository Institutions carry the highest systemic exposure.

## Project Structure

```
DA02_Credit-Report-Analysis/
├── README.md
├── DA02_Project-Plan.md         # Full problem statement and methodology
├── analysis.ipynb               # Stage 1: 5-stage EDA pipeline
├── final-report.ipynb           # Stage 2: Narrative report (Size → Rank → Explain → Compare → Recommend)
├── index.html                   # Stage 3: Self-contained editorial HTML report
├── data/
│   ├── FRB_G19_(2).csv          # Raw Federal Reserve data (50 series × 61 months)
│   └── credit_analysis_final.csv # Processed output (1,586 rows × 15 columns)
└── charts/
    ├── A2_coverage_by_era.png
    ├── B1_value_distributions.png
    ├── B2_era_heatmap.png
    ├── B3_correlation_matrix.png
    ├── B4_credit_type_comparison.png
    ├── B5_revolving_vs_growth.png
    ├── B6_holder_top_bottom.png
    ├── E1_stacked_area.png
    ├── E2_flow_rank.png
    ├── E3_yoy_trends.png
    ├── E4_revolving_compare.png
    └── E5_stress_matrix.png
```

## Dataset

| Property | Detail |
|----------|--------|
| **Source** | Federal Reserve G.19 Consumer Credit Report (Not Seasonally Adjusted) |
| **Period** | January 2020 – January 2025 (61 months) |
| **Raw Size** | 50 series × 61 monthly observations |
| **Processed Size** | 1,586 rows × 15 columns |
| **Holder Types** | Depository Institutions, Federal Government, Finance Companies, Credit Unions, Nonfinancial Business |

**Key Fields:** Series name, holder, credit_type (Total/Revolving/Nonrevolving), ownership_type (Owned/Securitized), measure (Level/Flow), value_millions, era, yoy_growth_pct, mom_growth_pct, rolling_3m_avg, holder_share_pct, revolving_share_pct.

## Data Processing

- **Unpivoting:** Wide-format Federal Reserve data reshaped to long-format (1,586 rows)
- **Era Classification:** COVID Shock, Recovery, Rate Hike Cycle, Post-Hike
- **Growth Metrics:** Year-over-year, month-over-month, and 3-month rolling average calculations
- **Outlier Detection:** 67 observations flagged (flow z-score > ±2.0); retained but explicitly marked
- **Share Computation:** Holder market share and revolving share calculated per period

## Project Includes

- Total credit outstanding tracking ($4.93T as of January 2025)
- Holder market share evolution across four economic eras
- Flow deterioration ranking by holder and era
- Year-over-year growth trends with era shading
- Revolving vs. nonrevolving credit flow comparison
- Composite stress scoring (volatility × recency × magnitude)
- Delinquency stress watchlist with actionable risk rankings

## Key Findings

1. **Total credit reached $4.93T** — up 18.6% vs. January 2020
2. **Federal Government shows policy-driven volatility** — student loan stress is orthogonal to macro cycles
3. **Revolving share hit cycle high of 26.3%** in December 2024 — negative flows since mid-2023 signal stress
4. **Credit Unions most stressed** (69% negative months in Post-Hike era) — Depository Institutions carry highest systemic exposure
5. **Non-bank lending outpacing banks** — Finance Companies +37.9%, Credit Unions +30.9% vs. Depository Institutions +16.4%

## Technologies Used

- **Python** — pandas, numpy, scipy, matplotlib, seaborn
- **Jupyter Notebook** — Reproducible analytical pipeline
- **HTML/CSS** — Self-contained editorial report with scroll animations, dark mode, and CSS-native charts

## How to Run

```bash
# 1. Install dependencies
pip install pandas numpy scipy matplotlib seaborn jupyter

# 2. Run the EDA pipeline (generates processed data + all charts)
jupyter nbconvert --to notebook --execute analysis.ipynb

# 3. Run the narrative report
jupyter nbconvert --to notebook --execute final-report.ipynb

# 4. Open the editorial report
open index.html
```

## Conclusion

This project reveals that **revolving credit stress is the leading indicator** for consumer delinquency risk in the post-rate-hike environment. The composite stress scoring framework surfaces Credit Unions and Depository Institutions as the top watchlist items, with credit card charge-off peaks projected for Q2–Q3 2025 based on the 6–12 month lag observed in flow deterioration patterns.

## Author

- **John**
- [GitHub](https://github.com/)
