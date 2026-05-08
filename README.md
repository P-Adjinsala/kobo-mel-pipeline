# 📋 KoboToolbox MEL Data Pipeline
### End-to-end Monitoring, Evaluation & Learning pipeline for field survey data

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/kobo-mel-pipeline/blob/main/kobo_mel_pipeline.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Compatible: KoboToolbox](https://img.shields.io/badge/Compatible-KoboToolbox%20API-orange.svg)](https://support.kobotoolbox.org/api.html)

---

## Overview

MEL (Monitoring, Evaluation & Learning) teams across Africa rely on KoboToolbox for field data collection — but the gap between raw submission data and programme-ready analysis remains a significant bottleneck. This project closes that gap with a **fully reproducible pipeline**: from KoboToolbox API pull to cleaned data, indicator computation, disaggregated analysis, and formatted Excel output, all in a single auditable notebook.

The working example simulates a **Maternal & Child Health (MCH) programme** across Cameroon's 10 regions, with 500 realistic survey submissions covering antenatal care, skilled delivery, vaccination, and postnatal check coverage.

---

## Pipeline stages

```
KoboToolbox API  →  Raw JSON  →  MELCleaner  →  Indicators  →  Charts  →  Excel export
      ↕                               ↕
  [Plug & play]             [Reusable class]
  [Simulated data                            ]
```

---

## What this project demonstrates

| Skill | Detail |
|-------|--------|
| **API integration** | KoboToolbox REST API with pagination — plug & play with real credentials |
| **Software design** | Reusable `MELCleaner` class with chainable methods and audit log |
| **Data quality** | Systematic duplicate detection, range validation, missing value imputation |
| **MEL indicators** | Coverage rates computed against logframe targets |
| **Disaggregation** | Region × urban/rural breakdowns for all four core indicators |
| **Reporting** | Formatted 4-sheet Excel workbook ready for donor/government sharing |

---

## MELCleaner — reusable across projects

```python
cleaner = (
    MELCleaner(df)
    .check_duplicates(id_col='_id')
    .validate_range('age', min_val=15, max_val=49)
    .fill_missing('age', strategy='median')
    .parse_dates('submission_date')
)
df_clean = cleaner.report()
# > Duplicates removed    : 15
# > Out-of-range [age]    : 20 set to nan
# > Missing filled [age]  : 20 using median (27.0)
# > Date parsed: month & quarter columns added
```

---

## Core indicators tracked

| Indicator | Target | Computation |
|-----------|--------|-------------|
| ANC 4+ Coverage | 75% | % respondents with 4+ antenatal visits |
| Skilled Delivery Rate | 80% | % deliveries attended by skilled personnel |
| Full Vaccination Rate | 70% | % children fully vaccinated |
| Postnatal Check Rate | 65% | % mothers with postnatal follow-up |

---

## Visualisations

- Bullet chart — actual vs target for all 4 indicators
- Regional disaggregation heatmap
- Urban vs Rural comparison (with target lines)
- Monthly trend chart with dual-axis submission volume

---

## Using with your real KoboToolbox project

```python
KOBO_TOKEN  = 'your_api_token'
FORM_UID    = 'your_form_uid'

df = fetch_kobo_data(KOBO_TOKEN, FORM_UID)
# All subsequent pipeline steps work identically
```

Replace `INDICATOR_COLS` and `TARGETS` in Section 6 with your programme's logframe indicators.

---

## Quick start

```bash
pip install pandas matplotlib seaborn openpyxl xlsxwriter requests
jupyter notebook kobo_mel_pipeline.ipynb
```

---

## Planned extensions

- Automated PDF report generation (`reportlab`)
- Enumerator-level anomaly detection (GPS drift, speed flags)
- Streamlit live dashboard for programme teams
- Multi-round comparison (baseline, midline, endline)

---

**Author:** Pantouin Adjinsala · University Lecturer & Civic Tech Contributor, AfricTivistes CitizenLab Cameroon  
**Built from:** Real MEL experience with KoboToolbox across civic and health programmes in Cameroon  
**Part of:** [AI for Social Good portfolio](https://github.com/YOUR_USERNAME)
