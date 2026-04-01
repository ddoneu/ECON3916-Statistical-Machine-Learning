# Lab 16: High-Dimensional GDP Growth Forecasting with Regularization

## Overview

This project demonstrates the bias-variance tradeoff in cross-country GDP growth prediction using World Bank Development Indicators (WDI). With ~28 predictors and ~166 training observations, OLS badly overfits — we apply Ridge and Lasso regularization to diagnose and fix this problem.

## Objectives

1. **Demonstrate the OLS failure mode** — overfitting when the predictor-to-observation ratio (p/n) is large
2. **Implement RidgeCV and LassoCV** using scikit-learn Pipelines with proper standardization
3. **Visualize the Lasso Path** — trace which indicators Lasso selects first as the penalty decreases
4. **Distinguish predictive redundancy from economic irrelevance** — a critical concept for applied economics

## Data

- **Source:** World Bank World Development Indicators (WDI) via the `wbgapi` Python package
- **Outcome:** Average annual GDP per capita growth (%), 2013–2019 (pre-COVID)
- **Predictors:** 28 indicators covering trade, macroeconomics, education, infrastructure, health, finance, natural resources, agriculture, and governance
- **Observations:** ~238 countries after cleaning, split 70/30 into train (~166) and test (~72) sets

## Key Results

| Method          | λ*      | Non-zero Predictors | Training R² | Test R² | Test MSE |
|-----------------|---------|---------------------|-------------|---------|----------|
| OLS             | N/A     | 28                  | 0.600       | -0.849  | 8.252    |
| Ridge (RidgeCV) | 47.1487 | 28                  | 0.557       | -0.051  | 4.691    |
| Lasso (LassoCV) | 0.0656  | 17                  | 0.569       | -0.330  | 5.934    |

- **OLS overfits dramatically** — Training R² of 0.600 but Test R² of -0.849 (worse than predicting the mean)
- **Ridge performs best** on test data, consistent with theory when predictors are highly correlated
- **Lasso selects 17 of 28 predictors**, zeroing out 11 redundant variables — useful for interpretability
- All models have negative test R², reflecting the genuine difficulty of cross-country growth prediction with noisy indicators

## Project Structure

```
├── lab_16_regularization.ipynb   # Main notebook with all code, plots, and analysis
├── lasso_path_gdp_growth.png    # Lasso Path visualization
└── README.md                    # This file
```

## How to Run

### Prerequisites

- Python 3.10+
- Required packages: `wbgapi`, `scikit-learn`, `matplotlib`, `seaborn`, `numpy`, `pandas`

### Installation

```bash
pip install wbgapi scikit-learn matplotlib seaborn numpy pandas
```

### Execution

1. Open `lab_16_regularization.ipynb` in Jupyter Notebook or JupyterLab
2. Run **Kernel → Restart & Run All**
3. The notebook downloads WDI data live from the World Bank API (requires internet connection)

## Key Concepts

- **Bias-Variance Tradeoff:** OLS minimizes bias but explodes variance when p/n is large. Regularization introduces bias to reduce variance, improving out-of-sample prediction.
- **Ridge (L2):** Shrinks all coefficients toward zero but keeps all predictors. Handles multicollinearity well.
- **Lasso (L1):** Drives many coefficients to exactly zero, performing automatic feature selection. Useful for interpretability but can arbitrarily drop one of several correlated predictors.
- **Lasso Path:** Visualizes the trajectory of all coefficients as λ varies — the first predictor to enter the model is the single strongest signal.
- **Conditional Predictive Redundancy:** A variable zeroed out by Lasso is not necessarily economically irrelevant — it may be redundant *given* other correlated predictors already in the model.

## Course Context

Completed as part of **ECON 3916: Statistical & Machine Learning for Economics** at Northeastern University, D'Amore-McKim School of Business.

## Author

Dat Do

## License

This project is for academic purposes only.
