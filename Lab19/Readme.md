# Tree-Based Models — Random Forests

## Objective

Benchmark ensemble tree methods against penalized linear regression on the California Housing dataset to quantify the predictive gains from non-parametric flexibility and to identify which housing features drive median home values through complementary importance measures.

## Methodology

- **Data:** California Housing (20,640 observations, 8 features — income, geography, dwelling characteristics). Train/test split at 80/20 with a fixed random state for reproducibility.
- **Baseline Models:** Decision Tree Regressor (unpruned) and Ridge Regression (α = 1.0) fitted as lower and upper bias-variance reference points.
- **Random Forest Tuning:** GridSearchCV over `n_estimators` ∈ {100, 200, 300}, `max_depth` ∈ {10, 20, None}, and `max_features` ∈ {√p, p/2, p}. Scoring on 5-fold cross-validated R². Best configuration selected by held-out test performance.
- **Feature Importance:** Compared Mean Decrease in Impurity (MDI) — which is biased toward high-cardinality features — against permutation importance on the test set, which provides a model-agnostic robustness check.
- **Classification Extension:** Binarized the target at the median to frame the problem as a classification task. Compared Random Forest Classifier AUC against Logistic Regression to assess whether ensemble gains persist under a different loss function.
- **Interactive Dashboard:** Built a Plotly + ipywidgets dashboard with sliders for `n_estimators` (10–500) and `max_features` (1–8), updating three live panels: model comparison, feature importance rankings, and train-vs-test learning curves.

## Key Findings

- **Random Forest outperformed both baselines** on test R², confirming that the non-linear interaction structure in housing data rewards flexible estimators. Ridge Regression, while stable, underfit the geographic and income interaction effects.
- **Diminishing returns on tree count:** Test R² plateaued around 100–150 estimators; additional trees contributed marginal variance reduction at increasing computational cost — consistent with bagging theory.
- **max_features as a regularizer:** Restricting features per split (≈ √p) improved generalization by decorrelating trees, narrowing the train-test R² gap. Setting max_features = p (all features) produced correlated trees and wider overfitting.
- **Median Income dominated both importance measures,** but MDI overstated the contribution of geographic coordinates (Latitude, Longitude) relative to permutation importance — a known artifact of MDI's bias toward continuous, high-cardinality splits.
- **Classification AUC:** Random Forest Classifier achieved higher AUC than Logistic Regression, indicating that ensemble flexibility benefits both regression and classification framings of this dataset.

## Tools

Python · scikit-learn · Plotly · ipywidgets · NumPy · Google Colab
