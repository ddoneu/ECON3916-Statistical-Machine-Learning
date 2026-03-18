# The Architecture of Dimensionality: Hedonic Pricing & the FWL Theorem

## Objective
Estimate a multivariate hedonic pricing model on 2026 California real estate data and furnish an empirical proof of the Frisch-Waugh-Lovell theorem by demonstrating that manual residual partialling recovers coefficients identical to full OLS estimation.

---

## Data
**Source:** Zillow synthetic dataset — 2026 California residential market  
**Variables:** `Sale_Price`, `Property_Age`, `Distance_to_Tech_Hub`

---

## Methodology

- **Baseline OLS (bivariate):** Regressed `Sale_Price` on `Property_Age` alone to establish a naive coefficient subject to omitted variable bias.
- **Multivariate OLS:** Extended the model to include `Distance_to_Tech_Hub`, producing the full-information benchmark coefficients.
- **FWL Residual Extraction:** Partialled out the influence of `Distance_to_Tech_Hub` from both `Sale_Price` and `Property_Age` by regressing each on the excluded covariate and retaining the residuals — isolating the variation in each variable that is orthogonal to tech hub proximity.
- **Auxiliary OLS on Residuals:** Regressed the `Sale_Price` residuals on the `Property_Age` residuals. Per the FWL theorem, this coefficient must equal the multivariate OLS estimate for `Property_Age`.
- **Verification:** Confirmed numerical equivalence between the partialledout coefficient and the full-model estimate, validating the theorem computationally.

---

## Key Findings

**Omitted Variable Bias (OVB):** The bivariate model suffered severe OVB. Excluding `Distance_to_Tech_Hub` caused the coefficient on `Property_Age` to absorb the shared covariance between age and proximity — both variables correlate with desirable location characteristics in California's tech-corridor markets. The result was a materially inflated (and directionally misleading) age coefficient, falsely attributing premium valuation to the physical structure rather than its proximity to economic activity.

**FWL Proof:** Manually isolating residuals via the partialling-out procedure stripped the confounded covariance from the age variable. The auxiliary regression on orthogonalized residuals produced a coefficient on `Property_Age` that matched the multivariate estimate exactly — an empirical demonstration of algorithmic *ceteris paribus*. The FWL theorem is not merely a theoretical identity; it is the mechanism by which OLS enforces conditional independence across regressors, and this lab makes that mechanism legible.

---

## Stack
`Python 3.10+` · `pandas` · `statsmodels.formula.api` · `matplotlib`
