# Data Dictionary

This document explains the main variables used in the **Hospital VBP Financial Disparities** project.

The project combines CMS HCRIS hospital cost report data, Medicare Value-Based Purchasing adjustment factors, county FIPS mapping, and CDC/ATSDR Social Vulnerability Index data.

---

## Main Analysis Dataset

The final analytic dataset is created by merging:

```text
CMS HCRIS hospital financial data
+ FY2021 Medicare VBP adjustment factors
+ national county FIPS crosswalk
+ CDC/ATSDR SVI 2020 county data
```

The analysis is restricted to **FY2021 short-term hospitals** with a valid **FY2021 VBP adjustment factor**.

---

## Key Variables

| Variable | Source | Meaning | Use in Analysis |
|---|---|---|---|
| `Provider CCN` | CMS HCRIS | Original hospital provider identifier. | Used to create cleaned CCN for merging. |
| `ccn` | Derived | Cleaned 6-digit hospital identifier. | Main merge key between HCRIS and VBP data. |
| `rpt_rec_num` | CMS HCRIS | Cost report record number. | Used for deduplication when multiple reports exist. |
| `Hospital Name` | CMS HCRIS | Hospital name. | Identification and review only. |
| `City` | CMS HCRIS | Hospital city. | Geographic context. |
| `State Code` | CMS HCRIS | Hospital state abbreviation. | Used for geographic analysis and state fixed effects. |
| `County` | CMS HCRIS | Hospital county name. | Used for FIPS and SVI matching. |
| `FIPS` | Derived / Crosswalk | Five-digit county FIPS code. | Merge key for CDC SVI data. |
| `Rural Versus Urban` | CMS HCRIS | Rural or urban hospital classification. | Used in t-tests, chi-square test, and regression controls. |
| `CCN Facility Type` | CMS HCRIS | Facility type classification. | Used to restrict analysis to short-term hospitals. |
| `Provider Type` | CMS HCRIS | Provider category. | Descriptive context. |
| `Type of Control` | CMS HCRIS | Hospital ownership/control category. | Used as a regression control. |
| `Fiscal Year Begin Date` | CMS HCRIS | Start date of cost reporting period. | Context for FY2021 filtering. |
| `Fiscal Year End Date` | CMS HCRIS | End date of cost reporting period. | Used to filter FY2021 reports. |
| `Medicare CBSA Number` | CMS HCRIS | Core-based statistical area code. | Geographic context. |
| `Number of Beds` | CMS HCRIS | Hospital bed count. | Hospital size variable. |
| `log_beds` | Derived | Log-transformed bed count. | Used in regression to reduce skewness. |
| `Total Patient Revenue` | CMS HCRIS | Total patient revenue. | Used for sensitivity/context. |
| `Net Patient Revenue` | CMS HCRIS | Revenue after deductions and adjustments. | Used to calculate operating margin. |
| `Less Total Operating Expense` | CMS HCRIS | Total operating expense. | Used to calculate operating margin. |
| `Net Income` | CMS HCRIS | Net income reported by the hospital. | Used for sensitivity/context. |
| `operating_margin` | Derived | Financial performance proxy. | Main outcome variable. |
| `margin_net_income` | Derived | Net income divided by total patient revenue. | Alternative margin measure for sensitivity/context. |
| `vbp_adjustment_factor_fy2021` | CMS VBP File | FY2021 Medicare Value-Based Purchasing adjustment factor. | Key explanatory variable and outcome in selected tests. |
| `RPL_THEMES` | CDC/ATSDR SVI | Overall county-level Social Vulnerability Index percentile ranking. | Main social vulnerability variable. |
| `SVI_missing` | Derived | Indicator for missing SVI after merge. | Data quality check. |
| `SVI_quartile` | Derived | Quartile grouping of `RPL_THEMES`. | Used in ANOVA and chi-square tests. |

---

## Main Outcome Variable

The main financial performance measure is:

```text
operating_margin
```

It is calculated as:

```text
Operating Margin = (Net Patient Revenue - Less Total Operating Expense) / Net Patient Revenue
```

This measures how much operating revenue remains after operating expenses.

Interpretation:

| Value | Meaning |
|---|---|
| Positive margin | Hospital has operating surplus. |
| Near zero | Hospital is near break-even. |
| Negative margin | Hospital has operating loss. |

Important note: operating margin is a proxy based on available HCRIS fields and should be interpreted carefully.

---

## Main Explanatory Variables

### `vbp_adjustment_factor_fy2021`

This variable represents the FY2021 Medicare Hospital Value-Based Purchasing adjustment factor.

General interpretation:

| Value | Meaning |
|---|---|
| Greater than 1.0 | Payment increase / positive adjustment. |
| Equal to 1.0 | Neutral adjustment. |
| Less than 1.0 | Payment reduction / negative adjustment. |

In this project, VBP adjustment factors are tightly centered around 1.0, so practical differences are usually small.

---

### `RPL_THEMES`

This is the overall Social Vulnerability Index ranking from the CDC/ATSDR SVI 2020 county file.

General interpretation:

| Value | Meaning |
|---|---|
| Closer to 0 | Lower social vulnerability. |
| Closer to 1 | Higher social vulnerability. |

This variable is measured at the **county level**, not the individual patient level.

---

### `Rural Versus Urban`

This field classifies hospitals as rural or urban.

It is used to compare financial performance and VBP adjustment patterns between geographic categories.

---

### `Number of Beds` and `log_beds`

`Number of Beds` measures hospital size.

Because bed count can be right-skewed, the analysis also uses:

```text
log_beds = log(1 + Number of Beds)
```

This transformation reduces skewness and helps regression modeling.

---

## Derived Variables

| Variable | Meaning |
|---|---|
| `ccn` | Cleaned 6-digit CCN used for merging HCRIS and VBP files. |
| `State_clean` | Standardized state abbreviation. |
| `County_key` | Normalized county name used for FIPS matching. |
| `FIPS` | County FIPS code used to merge SVI data. |
| `operating_margin` | Main hospital financial performance proxy. |
| `margin_net_income` | Alternative financial margin measure. |
| `SVI_missing` | Indicates whether SVI was missing after merge. |
| `SVI_quartile` | Groups SVI into Q1–Q4 for ANOVA and chi-square tests. |
| `log_beds` | Log-transformed hospital size variable. |

---

## Hypothesis Testing Variables

| Hypothesis Area | Variables Used |
|---|---|
| Rural vs urban margin comparison | `Rural Versus Urban`, `operating_margin` |
| Rural vs urban VBP comparison | `Rural Versus Urban`, `vbp_adjustment_factor_fy2021` |
| SVI quartile vs margin | `SVI_quartile`, `operating_margin` |
| SVI quartile vs VBP | `SVI_quartile`, `vbp_adjustment_factor_fy2021` |
| Rural/urban vs SVI quartile | `Rural Versus Urban`, `SVI_quartile` |
| SVI vs VBP correlation | `RPL_THEMES`, `vbp_adjustment_factor_fy2021` |
| VBP vs operating margin regression | `vbp_adjustment_factor_fy2021`, `operating_margin` |
| SVI vs operating margin regression | `RPL_THEMES`, `operating_margin` |
| Beds vs operating margin regression | `log_beds`, `operating_margin` |
| Controlled VBP model | `RPL_THEMES`, `log_beds`, `Rural Versus Urban`, `Type of Control`, `State Code`, `vbp_adjustment_factor_fy2021` |

---

## Data Quality Fields

| Field | Purpose |
|---|---|
| `vbp_match_rate` | Measures how many HCRIS hospitals matched to VBP data before restriction. |
| `fips_match_rate` | Measures how many hospitals matched to county FIPS codes. |
| `svi_non_missing_rate` | Measures how many hospitals had non-missing SVI data. |
| `svi_missing_pct` | Reports the percentage of missing SVI values. |

These fields are used to evaluate whether the merged analytic dataset is reliable enough for inference.

---

## Important Interpretation Notes

- The analysis is restricted to **short-term hospitals with valid FY2021 VBP adjustment factors**.
- `operating_margin` is a financial performance proxy, not a full measure of hospital financial health.
- `RPL_THEMES` is measured at the county level and does not describe individual patients.
- VBP adjustment factors are close to 1.0, so statistically significant differences may still be small in practical payment terms.
- The analysis is observational and cross-sectional, so results should be interpreted as associations, not causal effects.
- State-level differences and hospital characteristics may explain some relationships observed in simpler tests.
