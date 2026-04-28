# Geographic Disparities in Hospital Financial Performance Under Medicare Value-Based Purchasing

## Healthcare Analytics · Hypothesis Testing · Regression Analysis · Python

This project examines whether hospital financial performance in FY2021 is associated with Medicare Value-Based Purchasing adjustment factors, county-level social vulnerability, and geographic location.

The analysis focuses on **short-term hospitals with valid FY2021 Medicare VBP adjustment factors** and uses a full statistical workflow: data extraction, cleaning, merging, exploratory analysis, assumption checks, hypothesis testing, and regression modeling.

---

## Project Overview

Hospitals do not operate under the same financial conditions. Rural hospitals and hospitals serving socially vulnerable communities may face structural challenges that are not fully captured by performance-based payment programs.

This project investigates whether those geographic and community-level factors are associated with hospital operating margin and Medicare VBP adjustment factors.

The goal is not only to run statistical tests, but to show a clear healthcare analytics workflow that connects data preparation, statistical reasoning, and policy-relevant interpretation.

---

## Research Objective

This project was designed to answer three main questions:

1. Do rural and urban short-term hospitals differ in financial performance?
2. Is county-level social vulnerability associated with VBP adjustment factors or operating margin?
3. Does Medicare VBP meaningfully explain hospital financial performance after accounting for hospital and geographic characteristics?

---

## Data Sources

This project combines four data sources:

| Data Source | Purpose |
|---|---|
| CMS HCRIS Provider Cost Reports | Hospital financial and operational data |
| CMS FY2021 Hospital VBP Adjustment Factor File | Medicare VBP payment adjustment factors |
| CDC/ATSDR Social Vulnerability Index 2020 | County-level social vulnerability measure |
| National County FIPS Crosswalk | County/state to FIPS matching for SVI merge |

The notebook uses CMS HCRIS data, FY2021 VBP adjustment factors, the CDC/ATSDR SVI 2020 county file, and a county FIPS crosswalk. :contentReference[oaicite:0]{index=0}

---

## Study Scope

| Item | Scope |
|---|---|
| Fiscal Year | FY2021 |
| Facility Type | Short-Term Hospitals |
| Sample Restriction | Hospitals with valid FY2021 VBP adjustment factors |
| Main Outcome | Operating margin |
| Main Social Vulnerability Measure | CDC/ATSDR SVI `RPL_THEMES` |
| Statistical Focus | Hypothesis testing and regression analysis |

The analysis intentionally restricts the sample to short-term hospitals with valid FY2021 VBP adjustment factors because the research question is specifically about hospitals participating in the Medicare VBP payment mechanism. :contentReference[oaicite:1]{index=1}

---

## Repository Structure

```text
hospital-vbp-financial-disparities/
│
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
│
├── notebooks/
│   └── hospital_vbp_svi_analysis.ipynb
│
├── reports/
│   └── final_report.pdf
│
├── docs/
│   ├── methodology.md
│   ├── data_dictionary.md
│   └── hypothesis_tests_summary.md
│
└── data/
    └── README.md
```

---

## Methodology Summary

The project follows a structured statistical workflow:

1. Retrieve CMS HCRIS cost report data.
2. Filter to FY2021 fiscal year end dates.
3. Restrict the sample to short-term hospitals.
4. Clean hospital identifiers and financial fields.
5. Calculate operating margin as a financial performance proxy.
6. Deduplicate hospital records using CCN and report timing.
7. Merge FY2021 VBP adjustment factors.
8. Standardize county names and map hospitals to FIPS codes.
9. Merge county-level SVI data.
10. Run EDA, assumption checks, hypothesis tests, and regression models.

The technical notebook includes HCRIS extraction, FY2021 filtering, STH restriction, VBP merging, county/FIPS/SVI merging, QA checks, EDA, assumption checks, hypothesis testing, and regression analysis. :contentReference[oaicite:2]{index=2}

---

## Main Outcome Variable

The main financial performance measure is **operating margin**:

```text
Operating Margin = (Net Patient Revenue - Less Total Operating Expense) / Net Patient Revenue
```

Operating margin is used as a proxy for hospital financial performance.

Interpretation:

| Operating Margin | Meaning |
|---|---|
| Positive | Hospital has operating surplus |
| Near zero | Hospital is near break-even |
| Negative | Hospital has operating loss |

---

## Key Variables

| Variable | Description |
|---|---|
| `operating_margin` | Main hospital financial performance proxy |
| `vbp_adjustment_factor_fy2021` | Medicare VBP adjustment factor for FY2021 |
| `RPL_THEMES` | CDC/ATSDR Social Vulnerability Index overall percentile ranking |
| `Rural Versus Urban` | Rural or urban hospital classification |
| `Number of Beds` | Hospital size |
| `log_beds` | Log-transformed hospital size variable |
| `Type of Control` | Hospital ownership/control category |
| `State Code` | State-level control / fixed effect |

---

## Statistical Methods Used

| Method | Purpose |
|---|---|
| Descriptive Statistics | Understand sample distributions and central tendencies |
| Correlation Matrix | Explore bivariate relationships |
| Welch t-test | Compare rural vs urban hospitals |
| One-way ANOVA | Compare outcomes across SVI quartiles |
| Tukey HSD | Identify pairwise differences after significant ANOVA |
| Chi-square Test | Test association between rural/urban status and SVI quartile |
| Pearson Correlation | Measure linear relationship between SVI and VBP |
| OLS Regression with HC3 SE | Estimate relationships with robust inference |
| Fixed Effects | Control for state-level differences |

---

## Hypothesis Testing Summary

| Hypothesis | Test | Result | Decision |
|---|---|---|---|
| H1: Rural vs Urban Operating Margin | Welch t-test | p = 0.0038 | Reject H0 |
| H2: Rural vs Urban VBP Adjustment | Welch t-test | p = 0.5025 | Fail to reject H0 |
| H3: SVI Quartile vs Operating Margin | One-way ANOVA | p = 0.6098 | Fail to reject H0 |
| H4: SVI Quartile vs VBP Adjustment | One-way ANOVA + Tukey HSD | p = 0.0012 | Reject H0 |
| H5: Rural/Urban vs SVI Quartile | Chi-square test | p = 0.0438 | Reject H0 |
| H6: SVI vs VBP Adjustment | Pearson correlation | r = -0.154, p = 0.0012 | Reject H0 |
| H7: VBP vs Operating Margin | OLS-HC3 | p = 0.1459 | Fail to reject H0 |
| H8: SVI vs Operating Margin | OLS-HC3 | p = 0.6425 | Fail to reject H0 |
| H9: Hospital Size vs Operating Margin | OLS-HC3 | p = 0.1707 | Fail to reject H0 |
| H10: SVI vs VBP Controlled Model | Multivariable OLS-HC3 | p = 0.9188 | Fail to reject H0 |

The final report presents the hypothesis testing results, including significant rural/urban operating margin differences, significant SVI quartile differences in VBP adjustment factors, and a weak negative SVI-VBP correlation. :contentReference[oaicite:3]{index=3}

---

## Key Findings

### 1. Rural hospitals showed lower operating margins

Rural hospitals had significantly lower operating margins than urban hospitals. This suggests that rural hospitals may face stronger financial pressure and structural disadvantages.

### 2. VBP adjustment factors did not differ significantly by rural/urban status

The analysis did not find a statistically significant difference in average VBP adjustment factors between rural and urban hospitals. This suggests that rural hospitals were not directly receiving significantly different VBP adjustment factors on average.

### 3. Social vulnerability was weakly associated with VBP in bivariate analysis

SVI had a weak but statistically significant negative correlation with VBP adjustment factors. Hospitals in more socially vulnerable counties tended to receive slightly lower VBP adjustment factors, but the relationship was weak.

### 4. The SVI-VBP relationship did not remain significant after controls

After controlling for hospital size, rural/urban classification, ownership type, and state effects, SVI was no longer a significant predictor of VBP adjustment factor. This suggests that the simple SVI-VBP relationship may be explained by other hospital or geographic characteristics.

### 5. VBP adjustment factors did not strongly explain operating margin

The bivariate regression did not show a statistically significant relationship between VBP adjustment factor and operating margin. This suggests that VBP payment adjustments alone do not meaningfully explain hospital financial performance.

### 6. Operating margin is influenced by broader structural factors

The findings suggest that hospital financial performance is shaped by a wider set of conditions, including geography, community context, hospital characteristics, and state-level differences.

---

## Regression Analysis

The project uses OLS regression with HC3 robust standard errors to improve inference reliability when assumptions are imperfect.

The main regression framework includes:

```text
Operating Margin ~ VBP Adjustment Factor + SVI + log(Beds) + Rural/Urban + Type of Control + State Fixed Effects
```

The final report explains that the adjusted regression model captures a meaningful share of variation in operating margin, while also noting that many important financial drivers remain outside the model. :contentReference[oaicite:4]{index=4}

---

## Assumption Checks

The notebook includes formal assumption checks before statistical interpretation.

Checks include:

- Shapiro-Wilk tests for normality
- Levene’s tests for equal variance
- QQ plots
- Chi-square expected count checks
- Scatterplots for correlation assumptions
- Residuals versus fitted plots
- Regression residual QQ plots
- Variance Inflation Factor checks
- Breusch-Pagan tests
- HC3 robust standard errors for regression inference

The project uses robust methods where assumptions are imperfect, especially for regression models and rural/urban comparisons. :contentReference[oaicite:5]{index=5}

---

## Project Deliverables

| Deliverable | Description |
|---|---|
| `notebooks/hospital_vbp_svi_analysis.ipynb` | Full technical notebook with code, EDA, tests, and regression |
| `reports/final_report.pdf` | Written healthcare analytics report |
| `docs/methodology.md` | Methodology documentation |
| `docs/data_dictionary.md` | Variable definitions and interpretation notes |
| `docs/hypothesis_tests_summary.md` | Clean summary of all statistical tests |
| `data/README.md` | Data source and reproducibility notes |

---

## How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/hospital-vbp-financial-disparities.git
cd hospital-vbp-financial-disparities
```

### 2. Install required packages

```bash
pip install -r requirements.txt
```

### 3. Add required source files

Place the required local files in the project directory using the filenames expected by the notebook:

```text
TABLE 16B - ACTUAL HOSPITAL VBP ADJUSTMENT FACTORS FOR FY 2021.xlsx
SVI_2020_US_county.csv
national_county.txt
```

The notebook retrieves CMS HCRIS data through the CMS API and may create local cache/output folders.

### 4. Run the notebook

Open and run:

```text
notebooks/hospital_vbp_svi_analysis.ipynb
```

---

## Data Availability

Large raw and processed data files are not included in this repository.

Excluded files may include:

```text
CMS HCRIS raw extract files
VBP Excel source file
SVI county CSV file
County crosswalk file
Cached parquet files
Large processed CSV outputs
Generated logs
Generated QA files
```

See [`data/README.md`](data/README.md) for details.

---

## Limitations

This project is an observational, cross-sectional analysis. Results should be interpreted as associations, not causal effects.

Important limitations:

- Operating margin is a proxy based on available HCRIS financial fields.
- VBP adjustment factors are tightly centered around 1.0, so practical differences may be small.
- SVI is measured at the county level, not the patient level.
- The analysis does not include all possible drivers of hospital financial performance, such as payer mix, case mix, service line mix, staffing costs, market competition, or capital structure.
- State-level differences and hospital-specific characteristics may explain some observed relationships.
- The sample is restricted to short-term hospitals with valid FY2021 VBP adjustment factors.

---

## Why This Project Matters

This project demonstrates how healthcare analytics can move beyond simple reporting and into statistical reasoning.

It connects financial data, payment policy, and community-level vulnerability to evaluate whether hospitals face unequal financial conditions under a value-based payment environment.

For a data analytics portfolio, this project shows the ability to:

- work with public healthcare datasets,
- clean and merge multi-source data,
- define research questions and hypotheses,
- check statistical assumptions,
- select appropriate statistical tests,
- use robust regression methods,
- interpret results carefully,
- and communicate findings in a healthcare policy context.

---

## Skills Demonstrated

- Healthcare data analytics
- Public data extraction and cleaning
- Multi-source data integration
- Statistical hypothesis testing
- Regression modeling
- Assumption checking
- Robust standard errors
- Healthcare policy interpretation
- Python-based analytical workflow
- Technical documentation
- Research-style reporting

---

## Project Status

Completed:

- Data extraction and cleaning
- FY2021 short-term hospital sample construction
- VBP adjustment factor merge
- County FIPS and SVI merge
- Exploratory data analysis
- Assumption checks
- Ten hypothesis tests
- Regression analysis with HC3 robust standard errors
- Final written report

---

## Author

Created as a healthcare analytics portfolio project focused on hospital finance, Medicare Value-Based Purchasing, social vulnerability, and statistical analysis.
