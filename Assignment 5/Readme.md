# Assignment 5: The Sovereign Risk Engine

## Objective

Built a next-generation Early Warning System (EWS) for the IMF Global Financial Stability Division to identify emerging-market economies at elevated risk of economic crisis, using regularization, logistic classification, and cost-sensitive threshold optimization on World Bank development indicators.

## Data

- **Source:** World Bank World Development Indicators (WDI) via the `wbgapi` Python package
- **Coverage:** 238 countries, 28 predictors across trade, macroeconomics, education, infrastructure, health, finance, natural resources, agriculture, and governance
- **Period:** 2013-2019 country-level averages (pre-COVID)
- **Outcome (continuous):** Average annual GDP per capita growth rate (%)
- **Outcome (binary):** Crisis = 1 if average growth < 0 (sustained economic contraction); base rate ~16%

## Methodology

**Phase 1 — Regularization and the Complexity Trap**
- Demonstrated OLS overfitting on a high-dimensional cross-country dataset (p/n = 0.169), with Training R² = 0.60 but Test R² = -0.91
- Applied RidgeCV and LassoCV with 5-fold cross-validation; Ridge achieved the best test performance (Test R² = -0.07), consistent with theory under multicollinearity
- Visualized the full Lasso coefficient path, identifying inflation_cpi as the first predictor to enter the model (strongest unconditional signal)
- Lasso selected 18 of 28 predictors, zeroing out redundant indicators

**Phase 2 — From Forecasting to Classification**
- Exposed the Linear Probability Model failure: 14 of 72 test-set predictions fell outside [0, 1]
- Fitted logistic regression on Lasso-selected features; computed odds ratios for all 18 predictors
- Identified arable_land_pct (OR = 0.39) and population_growth (OR = 2.28) as the strongest crisis predictors
- Produced side-by-side LPM vs. logistic sigmoid visualization

**Phase 3 — Operational Deployment and Metrics**
- Demonstrated the accuracy paradox: naive baseline achieves 80.6% accuracy with 0% crisis recall
- Generated confusion matrix, classification report, ROC curve (AUC = 0.74), and Precision-Recall curve (PR-AUC = 0.35)
- Conducted threshold sweep under a 5-mission capacity constraint (τ = 0.88, Recall = 14.3%) and identified the F1-optimal threshold (τ = 0.13, Recall = 71.4%)

**Phase 4 — AI-Assisted Diagnostics (P.R.I.M.E. Framework)**
- Bootstrap Lasso stability analysis (200 resamples): 9 predictors selected >80% of the time; life_expectancy selected only 17%, confirming predictive redundancy with correlated health/development indicators
- Cost-sensitive threshold optimization ($50B per missed crisis, $2M per false alarm): cost-minimizing threshold τ = 0.03, reflecting the extreme asymmetry in error costs

## Key Results

| Model | λ* | Non-zero Predictors | Training R² | Test R² | Test RMSE |
|-------|-----|---------------------|-------------|---------|-----------|
| OLS | N/A | 28 | 0.603 | -0.914 | 2.923 |
| Ridge | 47.15 | 28 | 0.559 | -0.068 | 2.183 |
| Lasso | 0.066 | 18 | 0.573 | -0.355 | 2.459 |

| Threshold | τ | Countries Flagged | Precision | Recall | Optimizes |
|-----------|------|-------------------|-----------|--------|-----------|
| F1-optimal | 0.13 | 25 | 40.0% | 71.4% | Harmonic mean of P and R |
| Capacity-constrained | 0.88 | 5 | 40.0% | 14.3% | Max 5 missions/quarter |
| Cost-minimizing | 0.03 | ~all | Low | ~100% | Expected dollar cost |

## Tech Stack

Python 3.10+, scikit-learn (LinearRegression, RidgeCV, LassoCV, LogisticRegression, lasso_path, StandardScaler, confusion_matrix, roc_curve, precision_recall_curve), wbgapi, statsmodels, matplotlib, seaborn, pandas, numpy

## How to Run

1. Open `Assignment_5_The_Sovereign_Risk_Engine.ipynb` in Google Colab
2. Run Kernel > Restart & Run All
3. The notebook downloads WDI data live from the World Bank API (requires internet connection)

## Project Structure

```
├── Assignment_5_The_Sovereign_Risk_Engine.ipynb   # Complete notebook
└── README.md                                       # This file
```
