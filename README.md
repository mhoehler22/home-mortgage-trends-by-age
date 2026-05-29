# Home Mortgage Trends by Age
### Exploratory Data Analysis of 2023 HMDA Loan Applications by Applicant Age Group

---

## Overview

This project analyzes the 2023 Home Mortgage Disclosure Act (HMDA) dataset — over 11 million loan applications — to explore whether elder mortgage applicants (age 62+) behave differently from younger applicants when purchasing a home. A newly added boolean field in the 2023 HMDA release flags applicants over age 62, which prompted the central question: do elder buyers "move down" into less expensive homes, taking out smaller loans and putting more equity down?

The analysis filters to principal-residence home purchase loans (~5.9 million records), then works through histograms, CDFs, scatterplots, a hypothesis test, and a regression model to characterize the relationship between applicant age and loan behavior.

**Data:** The HMDA Modified Loan/Application Register (LAR) is streamed directly from the CFPB's public S3 bucket at runtime — no local data file is required.

---

## Pipeline

All analysis runs in a single notebook across nine sequential sections:

| # | Section | What It Does |
|---|---|---|
| 1 | Selecting Data | Stream 2023 HMDA LAR from CFPB S3; filter to principal-residence home purchases |
| 2 | Variable Description | Document and type-cast the 12 columns used in analysis |
| 3 | Histograms & Stats (5 variables) | Distributions and summary stats for Age, Income, Loan Term, Gender, and Loan Amount |
| 4 | PMF / CDFs | PMF and CDF of age groups; income CDF comparing originated vs. incomplete applications |
| 5 | CDF: Loan Amount | Compare CDF of loan amounts for elder (62+) vs. younger applicants |
| 6 | CDF: Property Value | Compare CDF of property values for elder vs. younger applicants |
| 7 | Scatterplots & Correlation | Pearson and Spearman correlation between age, income, loan amount, and LTV ratio |
| 8 | Hypothesis Test | Permutation test + t-test on difference in mean property values (homes ≤ $300K) |
| 9 | Regression Analysis | OLS linear and quadratic models; age + income predicting loan amount |

---

## Key Findings

- **Age distribution:** The 25–34 age group submits the most mortgage applications; the 30-year term is by far the most popular (followed by 25- and 15-year).
- **Elder vs. younger loan amounts:** CDFs show a clear shift — elder applicants consistently take out smaller loans, consistent with the "moving down" hypothesis.
- **Property value:** CDFs for elder and younger applicants are nearly identical, with a small but statistically significant divergence at values under $300K (p ≈ 0 by both permutation test and t-test).
- **Age–LTV correlation:** Pearson r = −0.316 between applicant age and combined loan-to-value ratio — a stronger relationship than age vs. loan amount alone.
- **Regression:** A linear model for age vs. loan amount yields R² = 0.001; adding a quadratic age term raises it to 0.026, confirming the expected "rise then fall" pattern across the lifespan. Adding income and income² pushes R² to **0.28** — age and income together explain 28% of loan amount variability, with all predictors significant at p < 0.001.

---

## Project Structure

```
home_mortgage_trends_by_age/
├── home_mortgage_trends_by_age.ipynb   # Full analysis notebook (9 sections)
├── column_head_mlar.docx               # HMDA LAR column header reference
├── field_list.xlsx                     # HMDA data dictionary (field descriptions)
├── Final Project PDF - DSC530 - Hoehler.pdf  # Rendered PDF of the notebook
├── requirements.txt
└── .gitignore
```

> **Note:** No data file is included. The notebook streams the 2023 HMDA LAR directly from the CFPB public S3 bucket. `thinkstats2.py` and `thinkplot.py` (Allen Downey) are also downloaded at runtime from the [ThinkStats2 GitHub repository](https://github.com/AllenDowney/ThinkStats2).

---

## Requirements

```
pip install -r requirements.txt
```

| Package | Purpose |
|---|---|
| numpy | Numerical operations |
| pandas | Data loading and filtering |
| matplotlib | Histograms and bar charts |
| scipy | t-test (`stats.ttest_ind`) |
| statsmodels | OLS regression (`smf.ols`) |

> `thinkstats2` and `thinkplot` are downloaded automatically by the notebook's first cell.

---

## Technologies

| Tool | Role |
|---|---|
| Python 3 | Analysis language |
| Jupyter Notebook | Interactive development environment |
| HMDA LAR (CFPB) | Primary data source (~11M rows, 2023) |
| ThinkStats2 (Downey) | CDF, Hist, and hypothesis test utilities |
| statsmodels | OLS regression |
| scipy | Statistical testing |

---

## Academic Context

This project was the term project for **DSC530: Data Exploration and Analysis** at Bellevue University (Spring 2024). It applies the exploratory data analysis methods from the course textbook — *Think Stats, 2nd Edition* by Allen B. Downey (O'Reilly, 2015) — to a real-world dataset of approximately 11 million mortgage records.

---

## Data Source

Consumer Financial Protection Bureau (2024). *Home Mortgage Disclosure Act: Modified Loan/Application Register (LAR)*. Federal Financial Institutions Examination Council (FFIEC). https://ffiec.cfpb.gov/data-publication/modified-lar/2023

---

*Analysis by Marty Hoehler — part of a portfolio of data science projects at [github.com/mhoehler22](https://github.com/mhoehler22).*
