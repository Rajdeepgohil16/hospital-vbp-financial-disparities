# Methodology

This document explains the methodology used in the **Hospital VBP Financial Disparities** project.

The project examines whether hospital financial performance in FY2021 is associated with Medicare Value-Based Purchasing adjustment factors, county-level social vulnerability, and geographic location.

---

## 1. Study Objective

The objective of this analysis is to evaluate whether short-term hospitals participating in Medicare Value-Based Purchasing show differences in financial performance based on:

- rural versus urban location,
- county-level social vulnerability,
- Medicare VBP adjustment factors,
- hospital size,
- ownership/control type,
- and state-level differences.

The analysis focuses on **FY2021 short-term hospitals with valid VBP adjustment factors**.

---

## 2. Research Context

Medicare Value-Based Purchasing is designed to connect hospital payment adjustments with performance measures. However, hospitals serving socially vulnerable or rural communities may face structural challenges that are not fully captured by performance-based payment formulas.

This project investigates whether those geographic and social vulnerability factors are associated with hospital financial performance.

---

## 3. Data Sources

The project combines four data sources:

| Data Source | Purpose |
|---|---|
| CMS HCRIS Provider Cost Reports | Provides hospital financial and operational data. |
| CMS FY2021 Hospital VBP Adjustment Factor File | Provides Medicare VBP adjustment factors for FY2021. |
| CDC/ATSDR Social Vulnerability Index 2020 | Provides county-level social vulnerability scores. |
| National County FIPS Crosswalk | Links hospital county/state information to county FIPS codes. |

---

## 4. Study Scope

The analysis applies the following scope restrictions:

- Fiscal year: FY2021
- Facility type: Short-Term Hospitals only
- Sample restriction: hospitals with valid FY2021 VBP adjustment factors
- Geography: U.S. hospitals with county/state information that can be linked to FIPS and SVI where available

The VBP-only restriction is intentional because the research question focuses on hospitals participating in the Medicare VBP payment mechanism.

---

## 5. Data Preparation Workflow

The notebook follows a structured data preparation process.

### Step 1: Extract HCRIS data

CMS HCRIS cost report data is retrieved using the CMS data API. The notebook uses pagination, retry logic, and local caching to reduce repeated API calls.

### Step 2: Filter to FY2021

Hospital records are filtered using fiscal year end dates from:

```text
2020-10-01 to 2021-09-30
```

This aligns the financial data with FY2021 VBP adjustment factors.

### Step 3: Restrict to short-term hospitals

The analysis keeps facilities classified as short-term hospitals. This creates a more comparable hospital peer group.

### Step 4: Clean financial fields

Financial fields such as net patient revenue, total operating expense, net income, total patient revenue, and number of beds are converted to numeric values.

Records with invalid or missing values needed for operating margin are removed.

### Step 5: Calculate operating margin

Operating margin is calculated as:

```text
Operating Margin = (Net Patient Revenue - Less Total Operating Expense) / Net Patient Revenue
```

Operating margin is used as the main financial performance proxy.

### Step 6: Remove extreme margin values

Operating margin values outside the range `[-1, 1]` are removed to reduce the impact of extreme reporting anomalies.

### Step 7: Deduplicate hospital reports

If more than one FY2021 report exists for the same hospital CCN, the latest fiscal year end date and report record number are used to keep one record per hospital.

### Step 8: Merge VBP data

FY2021 VBP adjustment factors are merged using cleaned hospital CCNs.

Hospitals without valid VBP adjustment factors are excluded from the final analytic sample because they are outside the VBP-focused study scope.

### Step 9: Standardize county names

County names are normalized to improve matching. This includes handling punctuation, abbreviations, suffixes such as “County” or “Parish,” and known naming variations.

### Step 10: Merge FIPS and SVI

County/state combinations are matched to FIPS codes using the national county crosswalk. The CDC/ATSDR SVI file is then merged by FIPS.

The key SVI measure is:

```text
RPL_THEMES
```

This represents the overall county-level Social Vulnerability Index percentile ranking.

---

## 6. Main Variables

| Variable | Role in Analysis |
|---|---|
| operating_margin | Main outcome variable for hospital financial performance. |
| vbp_adjustment_factor_fy2021 | Medicare VBP payment adjustment factor. |
| RPL_THEMES | County-level social vulnerability score. |
| Rural Versus Urban | Geographic classification of the hospital. |
| Number of Beds | Hospital size measure. |
| log_beds | Log-transformed hospital size variable. |
| Type of Control | Hospital ownership/control category. |
| State Code | State fixed effect/control variable. |

---

## 7. Exploratory Data Analysis

The exploratory analysis includes:

- descriptive statistics,
- missingness checks,
- operating margin distribution,
- VBP adjustment factor distribution,
- SVI distribution,
- correlation matrix,
- rural versus urban comparisons,
- SVI quartile comparisons,
- state-level margin summaries,
- and outlier review.

EDA is used to understand the data before formal statistical testing.

---

## 8. Hypothesis Testing Framework

The project tests ten hypotheses:

| Hypothesis | Topic | Method |
|---|---|---|
| H1 | Rural vs urban operating margin | Welch t-test |
| H2 | Rural vs urban VBP adjustment factor | Welch t-test |
| H3 | SVI quartile vs operating margin | One-way ANOVA |
| H4 | SVI quartile vs VBP adjustment factor | One-way ANOVA + Tukey HSD |
| H5 | Rural/urban status vs SVI quartile | Chi-square test |
| H6 | SVI vs VBP adjustment factor | Pearson correlation |
| H7 | VBP adjustment factor vs operating margin | OLS regression with HC3 SE |
| H8 | SVI vs operating margin | OLS regression with HC3 SE |
| H9 | Number of beds vs operating margin | OLS regression with HC3 SE |
| H10 | SVI vs VBP with controls | Multivariable OLS regression with HC3 SE |

All hypothesis tests use a significance level of:

```text
alpha = 0.05
```

---

## 9. Assumption Checks

Before interpreting statistical tests, the notebook checks key assumptions.

### T-test assumptions

For rural versus urban comparisons, the notebook checks:

- group sample sizes,
- Shapiro-Wilk normality tests,
- Levene’s test for equal variance,
- and QQ plots.

Welch’s t-test is used as a conservative choice.

### ANOVA assumptions

For SVI quartile comparisons, the notebook checks:

- group sizes,
- Shapiro-Wilk tests by quartile,
- Levene’s test across quartiles,
- and boxplots.

### Chi-square assumptions

For rural/urban status versus SVI quartile, the notebook checks expected cell counts.

### Correlation assumptions

For SVI versus VBP, the notebook uses scatterplots and outlier review to evaluate whether Pearson correlation is appropriate.

### Regression assumptions

For regression models, the notebook checks:

- residuals versus fitted values,
- QQ plot of residuals,
- Shapiro-Wilk test of residuals,
- variance inflation factors,
- and Breusch-Pagan test.

HC3 robust standard errors are used to improve inference reliability.

---

## 10. Regression Modeling

The main multivariable regression model evaluates operating margin using:

- VBP adjustment factor,
- SVI,
- log-transformed hospital beds,
- rural/urban classification,
- hospital ownership/control type,
- and state fixed effects.

The general model structure is:

```text
Operating Margin ~ VBP Adjustment Factor + SVI + log(Beds) + Rural/Urban + Type of Control + State Fixed Effects
```

HC3 robust standard errors are used because hospital financial data may contain skewness, outliers, and imperfect residual normality.

---

## 11. Interpretation Approach

The analysis separates:

- bivariate relationships,
- group differences,
- and adjusted regression findings.

This distinction is important because a variable may appear significant in a simple comparison but become insignificant after controlling for hospital characteristics and state differences.

The results are interpreted as evidence of association, not causation.

---

## 12. Key Methodological Cautions

Several cautions are important:

- Operating margin is a proxy based on available HCRIS fields.
- VBP adjustment factors are tightly centered around 1.0, so payment differences are usually small.
- SVI is measured at the county level, not at the patient level.
- The study is observational and cross-sectional.
- Results should not be interpreted as proving causal effects.
- State-level and hospital-level characteristics may explain part of the observed relationships.

---

## 13. Summary

This methodology combines healthcare financial data, payment adjustment data, geographic matching, and statistical testing to evaluate whether hospital financial performance differs by geography and community vulnerability.

The strength of the project is its complete analytical workflow:

```text
data extraction → cleaning → merging → EDA → assumption checks → hypothesis testing → regression interpretation
```

This makes the project useful as a healthcare analytics and statistical reasoning portfolio case study.
