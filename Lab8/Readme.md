# Hypothesis Testing & Causal Evidence Architecture  
**Dataset:** Lalonde (1986) — job training program evaluation  
**Focus:** Translating the scientific method into an applied workflow for adjudicating causal narratives

## Objective — From Estimation to Falsification
Most analyses stop at *estimating* an effect size. This project intentionally pivots to *falsification*: treating each causal claim as a testable hypothesis and using statistical contradiction to rule out narratives that don’t survive contact with the data.

Using the Lalonde (1986) dataset, I operationalized the scientific method to evaluate whether job training plausibly caused a measurable increase in earnings—framing the question as a competition between alternative explanations and then selecting the ones the data could not falsify.

## Technical Approach
- **Parametric ATE estimation (Signal-to-Noise):**
  - Computed a *Signal-to-Noise* ratio via **Welch’s T-Test** to estimate the **Average Treatment Effect (ATE)** on earnings.
  - Chose Welch’s formulation to remain robust under unequal variances and imbalanced group dispersion—common in earnings data.
- **Distribution-robust validation (Non-Parametric Inference):**
  - Ran a **non-parametric permutation test** with **10,000 resamples** to validate inference without relying on normality assumptions.
  - Used permutation-based null construction to stress-test conclusions against skewed/heavy-tailed income distributions.
- **Error control & decision discipline:**
  - Explicitly controlled **Type I error risk** by adhering to a consistent hypothesis-testing protocol (pre-specified test statistics, decision thresholds, and reporting).
  - Reported results in a falsification-forward format: *“What claims can we confidently reject?”* rather than *“What story fits best?”*
- **Tooling (Python scientific stack):**
  - Implemented both parametric and non-parametric testing workflows using **SciPy** to ensure reproducibility and standardization of statistical routines.

## Key Findings
- Observed a statistically significant increase in real earnings of approximately **+$1,795**, providing evidence inconsistent with the null hypothesis of no treatment effect.
- Rejected the null via **proof by statistical contradiction**: the observed effect was unlikely under the null model, and remained stable under non-parametric validation.

## Business Insight — Hypothesis Testing as the Safety Valve of the Algorithmic Economy
In modern data products, the biggest risk isn’t lack of data—it’s *false certainty*. When teams iterate quickly, run many experiments, and mine logs for “signals,” spurious correlations become a default failure mode.

Rigorous hypothesis testing acts as a **safety valve** for the algorithmic economy:
- **Prevents data grubbing:** forces explicit hypotheses and disciplined decision rules rather than post-hoc story fitting.
- **Reduces false positives:** protects roadmaps and model changes from being driven by noise that looks like impact.
- **Improves causal credibility:** separates “predictive convenience” from “decision-grade evidence,” which is critical when interventions affect users, budgets, or policy.
- **Makes analytics operational:** converts uncertainty into reliable go/no-go decisions, which is what businesses actually need.

This project demonstrates a pragmatic, production-minded stance: treat causality as a claim that must survive falsification, not a narrative we infer after the fact.

At Netflix, return-aware experimentation frames A/B tests as a decision-and-resource allocation problem: you design and decide experiments to maximize long-run returns to business metrics, not to “win” on statistical formality alone. This differs from the academic default of p < 0.05 (a convention optimized for strict false-discovery control), because a return-aware policy may intentionally relax p-value thresholds and tune decision rules when the upside of true positives and the opportunity cost of not shipping dominate. My portfolio takeaway: decision thresholds are business parameters—set by ROI, risk tolerance, and implementation costs—and then encoded into the experimentation system as policy, rather than treated as a universal scientific constant.
