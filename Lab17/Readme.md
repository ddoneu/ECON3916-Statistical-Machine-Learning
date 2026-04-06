# NY Fed Yield Curve Recession Model Replication

## Objective

Replicated the Federal Reserve Bank of New York's probit-based recession probability model using logistic regression on publicly available FRED macroeconomic data, estimating the likelihood of an NBER-defined recession 12 months ahead based solely on the Treasury yield curve spread.

## Methodology

- **Data Acquisition:** Downloaded the 10-Year minus 3-Month Treasury yield spread (T10Y3M) and the NBER recession indicator (USREC) from FRED via the `fredapi` Python library, covering January 1970 to present.
- **Data Construction:** Resampled the daily yield spread to month-end frequency, aligned it with the monthly recession indicator, and applied a 12-month lag to enforce a genuine out-of-sample forecasting structure consistent with the NY Fed's published methodology.
- **Linear Probability Model Baseline:** Fit an OLS regression on the binary recession outcome to demonstrate that the Linear Probability Model produces logically impossible predicted probabilities below 0 and above 1 — motivating the transition to logistic regression.
- **Logistic Regression:** Fit a single-predictor logistic regression using scikit-learn's `LogisticRegression`, extracted predicted recession probabilities via `.predict_proba()[:,1]`, and verified that all fitted values are bounded within [0, 1].
- **Odds Ratio Extraction:** Re-estimated the model using statsmodels `Logit` to obtain coefficient standard errors and 95% confidence intervals, then exponentiated to produce the odds ratio and its confidence bounds.
- **Probability Time Series:** Generated the full monthly recession probability series and visualized it with NBER recession shading in the style of the NY Fed's published charts.
- **Two-Predictor Extension:** Added the 12-month change in civilian unemployment rate (UNRATE), lagged 12 months, as a second predictor and compared odds ratios and statistical significance across models.
- **Cross-Validation:** Evaluated out-of-sample accuracy using `TimeSeriesSplit` (3-fold) to respect temporal ordering, and benchmarked against the naïve baseline accuracy of always predicting no recession.

## Tech Stack

Python · pandas · NumPy · scikit-learn (LogisticRegression, LinearRegression, TimeSeriesSplit, cross_val_score) · statsmodels (Logit) · matplotlib · Plotly · fredapi · ipywidgets

## Key Findings

- **LPM Failure on Real Data:** The Linear Probability Model produced out-of-bounds predictions on actual FRED observations, confirming that OLS is unsuitable for binary outcome modeling in applied macroeconomic forecasting.
- **Yield Spread Odds Ratio:** Each 1 percentage-point increase in the lagged yield spread reduced the odds of recession by approximately 50–55%, with a statistically significant odds ratio well below 1.0 and a tight 95% confidence interval.
- **2022–2024 Inversion:** The model assigned recession probabilities of 60–80% during the deepest and longest yield curve inversion since the 1980s. No NBER recession followed as of this writing — a reminder that elevated predicted probability is not equivalent to a certain outcome.
- **2006–2007 Early Warning:** The model flagged rising recession risk more than 12 months before the onset of the Great Recession in December 2007, demonstrating the yield curve's value as a leading indicator.
- **Two-Predictor Model:** Adding the 12-month change in unemployment had a negligible effect on the yield spread's odds ratio and was not statistically significant (p ≈ 0.56), suggesting the yield curve already captures most of the recession signal at a 12-month horizon.
- **Cross-Validation:** Neither the one-predictor nor two-predictor model exceeded the naïve baseline accuracy under `TimeSeriesSplit`, highlighting the limitations of raw accuracy as a metric under severe class imbalance (~7% recession rate in sample).
