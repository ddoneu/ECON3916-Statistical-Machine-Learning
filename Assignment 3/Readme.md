Assignment 3: The Causal Architecture
ECON 3916 | Statistical & Machine Learning for Economics | Module 3

Overview
This notebook was completed as part of the role of Senior Data Economist at SwiftCart Logistics, a multinational on-demand delivery platform. The executive board faced contradictory data on three key business questions: driver compensation equity, the efficacy of a new batch-routing algorithm, and the true ROI of the SwiftPass premium subscription service.
The mandate: cut through statistical noise using heavy computation and causal inference — discarding fragile parametric assumptions in favor of empirical evidence.

Phases
Phase 1 — Bootstrapping Non-Parametric Uncertainty

Simulated an audit sample of 250 driver tips combining zero-inflated and exponential distributions
Built a manual bootstrap engine (10,000 resamples) to estimate the 95% confidence interval for the median tip
Demonstrated and discussed CI asymmetry arising from the hard floor at $0 and unbounded right tail — a case where standard parametric CIs would be economically meaningless

Phase 2 — Falsification in Logistics A/B Testing

Generated synthetic A/B test data: Normal (control) vs Log-Normal with crash-loop outliers (treatment)
Built a manual permutation test (5,000 iterations) to compute an exact empirical p-value
Result: statistically significant difference at α = 0.05, validating the batch-routing algorithm's effect on delivery times

Phase 3 — Causal Control and Selection Bias Mitigation

Loaded real-world SwiftCart loyalty program data (swiftcart_loyalty.csv)
Computed the Naive Simple Difference in Outcomes (SDO): $17.57 — inflated by selection bias
Built a full Propensity Score Matching (PSM) pipeline:

Logistic Regression to estimate propensity scores
Nearest Neighbor 1:1 matching
ATT estimation via t-test on matched pairs


Recovered ATT: $10.14 — the true causal effect of SwiftPass after removing $7.43 of pure selection bias (42% of the naive SDO)

Phase 4 — AI Expansion: Love Plot Visualization

Generated via the P.R.I.M.E. Framework (LLM-assisted using Claude)
Produced a Love Plot showing Standardized Mean Differences before and after matching
Post-matching SMDs collapse within the |SMD| < 0.1 threshold, confirming successful covariate balance and validating the causal interpretation of the ATT


Tools & Libraries

numpy, pandas, matplotlib, seaborn
sklearn (LogisticRegression, NearestNeighbors)
scipy.stats (t-test)
