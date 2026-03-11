# Data Wrangling & Engineering Pipeline

## Objective
Architect a robust preprocessing pipeline to resolve structural data quality failures — including missingness patterns, multicollinearity, and high-cardinality categorical encoding — transforming a noisy HR economics dataset into a regression-ready analytical asset.

---

## Methodology

- **Missingness Diagnosis:** Employed `missingno` matrix and heatmap visualizations to classify the missingness regime across all features, distinguishing Missing at Random (MAR) mechanisms from structurally complete variables. Imputation strategies were assigned conditionally based on variable type and missingness dependency.

- **Feature Engineering:** Constructed derived structural features from raw HR attributes to improve signal density and align the dataset schema with econometric modeling assumptions.

- **Multicollinearity Mitigation:** Applied the standard dummy variable correction to a set of nominal categorical variables, explicitly dropping a reference class per group to eliminate perfect multicollinearity and ensure full-rank design matrices — a prerequisite for OLS identification.

- **High-Cardinality Encoding:** Replaced frequency-based and one-hot approaches for high-dimensional geographic variables with **Target Encoding** via `category_encoders`, collapsing sparse regional identifiers into continuous posterior mean estimates while preserving predictive signal and controlling dimensionality growth.

- **Pipeline Validation:** Confirmed the transformed dataset's structural integrity — zero residual missingness, non-singular covariance structure, and dtype consistency — prior to downstream modeling handoff with `statsmodels`.

---

## Key Findings

The pipeline successfully resolved all three primary data pathologies present in `messy_hr_economics.csv`:

1. **MAR Structure Confirmed:** Missingness was not random noise but exhibited systematic dependencies on observed covariates, validating conditional imputation over listwise deletion and reducing information loss.

2. **Dummy Variable Trap Bypassed:** Enforcing reference-class dropping across all categorical encodings guaranteed linear independence among regressors, preserving the invertibility of **X'X** required for OLS estimation.

3. **Dimensionality Controlled via Target Encoding:** Geographic variables with prohibitively high cardinality were successfully compressed into a single continuous feature per variable, retaining outcome-relevant variation while eliminating the sparse, high-variance columns that one-hot encoding would have introduced.

The resulting dataset is structurally sound, econometrically identified, and ready for causal inference or predictive modeling workflows.
