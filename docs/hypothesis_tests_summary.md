# Hypothesis Testing Summary

This document summarizes the ten hypothesis tests used in the **Hospital VBP Financial Disparities** project.

The project examines whether hospital financial performance in FY2021 is associated with:

- rural versus urban hospital location,
- Medicare Value-Based Purchasing adjustment factors,
- county-level social vulnerability,
- hospital size,
- and selected hospital/state controls.

The analysis uses a significance level of:

```text
alpha = 0.05
```

---

## Summary Table

| Hypothesis | Research Question | Test Used | Result | Decision |
|---|---|---|---|---|
| H1 | Do rural and urban hospitals differ in operating margin? | Welch t-test | p = 0.0038 | Reject H0 |
| H2 | Do rural and urban hospitals differ in VBP adjustment factor? | Welch t-test | p = 0.5025 | Fail to reject H0 |
| H3 | Does operating margin differ across SVI quartiles? | One-way ANOVA | p = 0.6098 | Fail to reject H0 |
| H4 | Does VBP adjustment factor differ across SVI quartiles? | One-way ANOVA + Tukey HSD | p = 0.0012 | Reject H0 |
| H5 | Is rural/urban status associated with SVI quartile? | Chi-square test | p = 0.0438 | Reject H0 |
| H6 | Is SVI correlated with VBP adjustment factor? | Pearson correlation | r = -0.154, p = 0.0012 | Reject H0 |
| H7 | Does VBP adjustment factor predict operating margin? | OLS regression with HC3 SE | p = 0.1459 | Fail to reject H0 |
| H8 | Does SVI predict operating margin? | OLS regression with HC3 SE | p = 0.6425 | Fail to reject H0 |
| H9 | Does hospital size predict operating margin? | OLS regression with HC3 SE | p = 0.1707 | Fail to reject H0 |
| H10 | Does SVI predict VBP adjustment after controls? | Multivariable OLS with HC3 SE | p = 0.9188 | Fail to reject H0 |

---

## H1: Rural vs Urban Operating Margin

### Null Hypothesis

Rural and urban hospitals have the same mean operating margin.

### Alternative Hypothesis

Rural and urban hospitals have different mean operating margins.

### Test Used

Welch t-test.

### Result

The result was statistically significant:

```text
p = 0.0038
```

### Decision

Reject the null hypothesis.

### Interpretation

Rural hospitals had significantly lower operating margins than urban hospitals. This suggests that rural hospitals may face stronger financial pressure, even within the same VBP-focused hospital sample.

---

## H2: Rural vs Urban VBP Adjustment Factor

### Null Hypothesis

Rural and urban hospitals have the same mean VBP adjustment factor.

### Alternative Hypothesis

Rural and urban hospitals have different mean VBP adjustment factors.

### Test Used

Welch t-test.

### Result

The result was not statistically significant:

```text
p = 0.5025
```

### Decision

Fail to reject the null hypothesis.

### Interpretation

There was no statistically significant difference in VBP adjustment factors between rural and urban hospitals. This suggests that, on average, rural hospitals were not receiving significantly different VBP adjustment factors compared with urban hospitals.

---

## H3: SVI Quartile vs Operating Margin

### Null Hypothesis

Mean operating margin is equal across all SVI quartiles.

### Alternative Hypothesis

At least one SVI quartile has a different mean operating margin.

### Test Used

One-way ANOVA.

### Result

The result was not statistically significant:

```text
p = 0.6098
```

### Decision

Fail to reject the null hypothesis.

### Interpretation

Operating margin did not differ significantly across SVI quartiles. In the bivariate group comparison, social vulnerability did not show a clear direct relationship with operating margin.

---

## H4: SVI Quartile vs VBP Adjustment Factor

### Null Hypothesis

Mean VBP adjustment factor is equal across all SVI quartiles.

### Alternative Hypothesis

At least one SVI quartile has a different mean VBP adjustment factor.

### Test Used

One-way ANOVA with Tukey HSD post-hoc testing.

### Result

The result was statistically significant:

```text
p = 0.0012
```

### Decision

Reject the null hypothesis.

### Interpretation

VBP adjustment factors differed significantly across SVI quartiles. The post-hoc test indicated that the main significant difference was between the least vulnerable quartile and a higher-vulnerability quartile. This suggests that social vulnerability is related to VBP adjustment patterns in the bivariate comparison, although the practical size of the difference is small because VBP factors are tightly centered around 1.0.

---

## H5: Rural/Urban Status vs SVI Quartile

### Null Hypothesis

Rural versus urban classification is independent of SVI quartile.

### Alternative Hypothesis

Rural versus urban classification is associated with SVI quartile.

### Test Used

Chi-square test of independence.

### Result

The result was statistically significant:

```text
p = 0.0438
```

### Decision

Reject the null hypothesis.

### Interpretation

Rural/urban classification was significantly associated with SVI quartile. This suggests that rural and urban hospitals are not evenly distributed across social vulnerability levels.

---

## H6: SVI vs VBP Adjustment Factor

### Null Hypothesis

There is no correlation between SVI and VBP adjustment factor.

### Alternative Hypothesis

There is a correlation between SVI and VBP adjustment factor.

### Test Used

Pearson correlation.

### Result

The result showed a weak but statistically significant negative correlation:

```text
r = -0.154
p = 0.0012
```

### Decision

Reject the null hypothesis.

### Interpretation

Higher social vulnerability was associated with slightly lower VBP adjustment factors. However, the relationship was weak, meaning SVI alone explains only a small part of the variation in VBP adjustments.

---

## H7: VBP Adjustment Factor vs Operating Margin

### Null Hypothesis

VBP adjustment factor is not associated with operating margin.

### Alternative Hypothesis

VBP adjustment factor is associated with operating margin.

### Test Used

OLS regression with HC3 robust standard errors.

### Result

The result was not statistically significant:

```text
p = 0.1459
```

### Decision

Fail to reject the null hypothesis.

### Interpretation

VBP adjustment factor did not significantly predict operating margin in the bivariate regression model. This suggests that VBP adjustments alone do not explain hospital financial performance in a meaningful way.

---

## H8: SVI vs Operating Margin

### Null Hypothesis

SVI is not associated with operating margin.

### Alternative Hypothesis

SVI is associated with operating margin.

### Test Used

OLS regression with HC3 robust standard errors.

### Result

The result was not statistically significant:

```text
p = 0.6425
```

### Decision

Fail to reject the null hypothesis.

### Interpretation

SVI alone did not significantly predict operating margin in the bivariate regression model. This means county-level social vulnerability did not show a direct standalone relationship with operating margin in this test.

---

## H9: Hospital Size vs Operating Margin

### Null Hypothesis

Hospital size is not associated with operating margin.

### Alternative Hypothesis

Hospital size is associated with operating margin.

### Test Used

OLS regression with HC3 robust standard errors.

Hospital size was measured using log-transformed number of beds.

### Result

The result was not statistically significant:

```text
p = 0.1707
```

### Decision

Fail to reject the null hypothesis.

### Interpretation

Hospital size did not significantly predict operating margin. Larger hospitals were not clearly more or less financially profitable based only on bed count.

---

## H10: SVI vs VBP Adjustment Factor After Controls

### Null Hypothesis

SVI has no effect on VBP adjustment factor after controlling for hospital and state characteristics.

### Alternative Hypothesis

SVI has a significant effect on VBP adjustment factor after controlling for hospital and state characteristics.

### Test Used

Multivariable OLS regression with HC3 robust standard errors.

Controls included:

- log-transformed number of beds,
- rural/urban classification,
- type of control,
- and state fixed effects.

### Result

The result was not statistically significant:

```text
p = 0.9188
```

### Decision

Fail to reject the null hypothesis.

### Interpretation

After controlling for hospital and state characteristics, SVI was no longer significantly associated with VBP adjustment factor. This suggests that the weak bivariate SVI-VBP relationship may be explained by other hospital or geographic factors rather than social vulnerability alone.

---

## Overall Interpretation

The strongest evidence from the hypothesis testing is that rural hospitals had significantly lower operating margins than urban hospitals. The analysis also found that SVI was weakly and negatively associated with VBP adjustment factors in bivariate testing.

However, after controlling for hospital characteristics and state effects, SVI was not a significant independent predictor of VBP adjustment. Operating margin was also not significantly explained by VBP adjustment factor, SVI, or hospital size in the simple regression models.

Overall, the hypothesis testing suggests that hospital financial performance is influenced by broader structural and geographic factors, and that Medicare VBP adjustment factors alone do not meaningfully explain differences in operating margin.

---

## Methodological Notes

- Welch t-tests were used for rural versus urban comparisons because they are robust for unequal group variances.
- ANOVA was used for comparing more than two SVI quartile groups.
- Tukey HSD was used after the significant ANOVA result for VBP adjustment factors.
- Chi-square testing was used for association between two categorical variables.
- Pearson correlation was used for the bivariate SVI-VBP relationship.
- OLS regression with HC3 robust standard errors was used to improve inference reliability when regression assumptions were imperfect.
- Results should be interpreted as associations, not causal effects.
