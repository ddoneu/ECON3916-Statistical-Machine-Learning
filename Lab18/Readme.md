# Fraud Detection Model Evaluation — Metrics that Matter

## Objective

Evaluate the performance of a logistic regression fraud classifier on severely imbalanced transaction data using decision-theoretic evaluation metrics — moving beyond accuracy to confusion matrices, Precision, Recall, F1-Score, ROC curves, Precision-Recall curves, and capacity-constrained threshold selection.

## Methodology

- **Data**: Kaggle Credit Card Fraud Detection dataset — 284,807 European credit card transactions with 492 confirmed frauds (0.172% positive class). Features V1–V28 are PCA-anonymized principal components; Amount is the only interpretable continuous feature.
- **Preprocessing**: Standardized the Amount feature via z-score scaling; applied stratified 80/20 train-test split to preserve class proportions across partitions.
- **Baseline Benchmark**: Established the accuracy paradox by demonstrating that a naïve all-negative classifier achieves 99.83% accuracy with exactly 0% fraud recall — confirming that accuracy is uninformative under extreme class imbalance.
- **Model Training**: Fit a logistic regression classifier (scikit-learn) and generated predicted fraud probabilities for the held-out test set.
- **Evaluation Suite**: Computed confusion matrices, per-class Precision, Recall, and F1-Score at the default threshold (τ = 0.5); plotted ROC and Precision-Recall curves with AUC summaries; swept thresholds from 0.01 to 0.99 to trace the full precision-recall tradeoff frontier.
- **Capacity-Constrained Threshold Selection**: Identified the operating point that maximizes recall subject to a real-world constraint of 500 daily fraud investigations — translating model output into an actionable business decision.
- **Expansion — Interactive Dashboard**: Built an ipywidgets-based interactive dashboard in Google Colab with a threshold slider, configurable dollar-cost assumptions (FN × cost_fn + FP × cost_fp), and a side-by-side comparison of Logistic Regression vs. Random Forest using both ROC-AUC and PR-AUC.

## Key Findings

- **Accuracy Paradox Confirmed**: A naïve baseline achieves 99.83% accuracy while detecting zero fraud — validating the need for class-sensitive evaluation metrics.
- **Strong Discriminative Power**: Logistic regression achieved high ROC-AUC, indicating that the model reliably ranks fraudulent transactions above legitimate ones in predicted probability.
- **PR-AUC as the Honest Test**: The Precision-Recall curve provided a more rigorous assessment of fraud-class performance than the ROC curve, which is inflated by the abundance of true negatives in imbalanced settings.
- **Threshold Sensitivity**: The F1-optimal threshold diverges substantially from the default τ = 0.5, demonstrating that threshold selection is a business decision — not a statistical default.
- **Capacity-Constrained Operating Point**: At τ = 0.01, the model flags 246 transactions (within the 500-investigation budget) and achieves 88.78% recall at 35.37% precision — a deployable configuration where the binding constraint is model quality, not team capacity.

## Tech Stack

Python · pandas · NumPy · scikit-learn · matplotlib · seaborn · ipywidgets · Google Colab
