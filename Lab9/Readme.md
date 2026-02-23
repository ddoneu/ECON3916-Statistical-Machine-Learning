# Recovering Experimental Truths via Propensity Score Matching  
**Dataset:** Lalonde (1986) — observational subset  
**Theme:** Causal identification under selection bias

## Objective
Demonstrate how Propensity Score Matching (PSM) can correct the “observational failure” of naive comparisons by explicitly modeling selection into treatment and reconstructing an experimental-quality counterfactual.

## Methodology
- **Diagnosed selection bias:** Treated treatment assignment as non-random and framed the problem as one of missing counterfactuals driven by systematic differences between treated and control units.
- **Modeled treatment assignment:** Fit **logistic regression propensity models** to estimate each individual’s probability of receiving training conditional on observed covariates.
- **Estimated propensity scores:** Generated propensity scores as a low-dimensional summary of covariate balance targets—enabling comparison of “like with like.”
- **Matched to construct counterfactuals:** Applied **nearest-neighbor matching** on propensity scores to pair treated individuals with observationally similar controls, approximating randomized assignment.
- **Validated identification logic:** Checked that matching reduced pre-treatment imbalance and improved the credibility of the overlap/common-support assumption (i.e., treated units had comparable controls).

## Key Findings
- The naive observational estimate implied a large negative effect (**≈ −$15,204**), reflecting heavy selection bias rather than program impact.
- After propensity modeling and nearest-neighbor matching, I recovered an effect consistent with the experimental benchmark (**≈ +$1,800**), demonstrating that properly engineered counterfactuals can restore causal signal in observational settings.
