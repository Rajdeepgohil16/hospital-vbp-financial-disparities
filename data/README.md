# Data Folder

This folder explains the data requirements for the **Hospital VBP Financial Disparities** project.

The full raw and processed datasets are **not included** in this repository because the files may be large and should be downloaded separately from their original public sources. This keeps the GitHub repository clean, lightweight, and easier to review.

---

## Project Data Overview

This project studies whether hospital financial performance in FY2021 is associated with:

- Medicare Value-Based Purchasing adjustment factors
- County-level social vulnerability
- Rural versus urban hospital location
- Hospital size and ownership characteristics

The analysis is restricted to **short-term hospitals (STH)** with a valid **FY2021 Medicare VBP adjustment factor**.

---

## Data Sources

This project combines four data sources:

| Data Source | Purpose |
|---|---|
| CMS HCRIS Provider Cost Reports | Provides hospital financial and operational data. |
| CMS FY2021 Hospital VBP Adjustment Factor File | Provides Medicare Value-Based Purchasing adjustment factors. |
| CDC/ATSDR Social Vulnerability Index 2020 | Provides county-level social vulnerability scores. |
| National County FIPS Crosswalk | Used to match hospital county/state information to county FIPS codes. |

---

## Required Local Files

To reproduce the analysis, download the required files and place them in the project directory using the same filenames referenced in the notebook.

Expected local files:

```text
TABLE 16B - ACTUAL HOSPITAL VBP ADJUSTMENT FACTORS FOR FY 2021.xlsx
SVI_2020_US_county.csv
national_county.txt
```

The notebook retrieves CMS HCRIS data through the CMS data API and may cache the downloaded data locally.

---

## Files Not Included in This Repository

The following types of files are intentionally excluded from GitHub:

```text
CMS HCRIS raw extract files
Cached parquet files
Large processed CSV outputs
VBP Excel source file
SVI county CSV file
County crosswalk text file
Generated log files
Generated QA JSON files
Generated figures
```

These files are excluded because they are either large, externally sourced, or generated during notebook execution.

---

## Generated Output Folders

When the notebook is run, it may create these local folders:

```text
cache/
data_out/
figures/
logs/
```

These folders are used for local processing only.

| Folder | Purpose |
|---|---|
| cache/ | Stores cached CMS HCRIS data to avoid repeated API downloads. |
| data_out/ | Stores cleaned analytic outputs. |
| figures/ | Stores charts generated during EDA and diagnostics. |
| logs/ | Stores QA checks and run-related output files. |

---

## Main Generated Output Files

The notebook may generate outputs such as:

```text
hospital_provider_cost_report_full.csv
hcris_fy2021_sth_margin_vbp_svi_final.csv
qa_checks.json
```

These files are not uploaded to GitHub because they are generated from the notebook and may be large.

---

## Reproducibility Steps

To reproduce the analysis:

1. Download the required source files.
2. Place the VBP Excel file, SVI county file, and county crosswalk file in the project directory.
3. Open the notebook in the `notebooks/` folder.
4. Run the notebook from top to bottom.
5. The notebook will retrieve CMS HCRIS data, process the files, merge datasets, run statistical tests, and generate results.

---

## Important Data Preparation Notes

The analysis applies the following restrictions and cleaning steps:

- Keeps hospitals with FY2021 fiscal year end dates.
- Restricts the sample to short-term hospitals.
- Keeps hospitals with valid net patient revenue and operating expense fields.
- Calculates operating margin as a financial performance proxy.
- Deduplicates hospital reports by keeping the latest report per CCN.
- Merges VBP adjustment factors using cleaned hospital CCNs.
- Standardizes county names for FIPS matching.
- Merges county-level SVI using FIPS codes.
- Treats missing or invalid SVI values as missing.

---

## Main Analysis Dataset

The final analytic dataset is built after merging:

```text
HCRIS hospital financial data
+ FY2021 VBP adjustment factors
+ county FIPS crosswalk
+ CDC/ATSDR SVI 2020
```

The final dataset is used for:

- Exploratory data analysis
- Assumption checks
- Hypothesis testing
- Regression modeling
- Healthcare policy interpretation

---

## Interpretation Notes

This project is a statistical healthcare analytics project, not a dashboard project.

The focus is on:

- hypothesis testing,
- assumption checking,
- regression modeling,
- statistical interpretation,
- and healthcare policy implications.

Important interpretation cautions:

- Operating margin is a proxy based on available HCRIS financial fields.
- VBP adjustment factors are centered close to 1.0, so payment differences are usually small.
- SVI is measured at the county level, not the individual patient level.
- The analysis is observational and should not be interpreted as proving causation.
- Results should be interpreted as evidence of association, not direct causal impact.

---

## Why Data Is Excluded

The raw and processed data files are not included because:

- they may exceed normal GitHub file-size limits,
- they can be downloaded from public sources,
- they are generated or cached during notebook execution,
- and excluding them keeps the repository easier to review.

The notebook and final report document the complete methodology, analysis process, and results.
