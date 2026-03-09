# Spurious Correlation & Multicollinearity in Macroeconomic Time-Series Data

**Tools:** Python · pandas · seaborn · statsmodels · FRED API · NetworkX (DAG)

---

## Overview

This project investigates a foundational pitfall in applied econometrics: the tendency of raw macroeconomic level data to produce misleading correlation structures that can compromise regression inference. Using five FRED indicators — CPI, Unemployment, Federal Funds Rate, Industrial Production, and M2 Money Supply — the analysis demonstrates how non-stationarity inflates pairwise correlations and how principled transformations restore interpretability.

---

## Methodology

**1. Correlation Diagnostics on Raw Levels**
Constructed a correlation heatmap using seaborn to visualize pairwise relationships across raw level variables. Near-unity correlations across structurally unrelated indicators revealed the presence of spurious correlation driven by shared secular trends rather than genuine economic co-movement.

**2. Multicollinearity Quantification via VIF**
Applied Variance Inflation Factor (VIF) diagnostics through statsmodels to measure regressor redundancy. Elevated VIF scores (>10) confirmed that the raw-level feature space was severely collinear, rendering coefficient estimates unstable and standard errors unreliable for any downstream regression model.

**3. YoY Transformation for Trend Removal**
Transformed all non-stationary series into Year-over-Year (YoY) growth rates to eliminate deterministic trends. Post-transformation correlation analysis demonstrated a marked reduction in spurious associations, with residual correlations reflecting cyclical co-movement consistent with established macroeconomic theory (e.g., the Phillips curve relationship between inflation and unemployment).

**4. Causal Structure Mapping via DAGs**
Constructed Directed Acyclic Graphs (DAGs) to make explicit the assumed structural relationships among variables — distinguishing direct causal pathways from confounded associations. This step grounded the statistical findings in economic theory and clarified which correlations survive transformation for substantive reasons versus those that were purely artifactual.

---

## Key Takeaways

- Raw macroeconomic levels are structurally non-stationary; correlation matrices built on them measure trend co-movement, not economic relationships.
- VIF diagnostics provide a quantitative threshold for identifying when multicollinearity poses a practical threat to model validity.
- YoY transformation is a lightweight, interpretable stationarity correction well-suited for business and policy contexts.
- DAGs bridge the gap between statistical patterns and causal economic reasoning, a critical step before any inferential modeling.
