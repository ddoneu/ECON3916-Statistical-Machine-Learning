## Lab 6: The Architecture of Bias (DGP, Sampling Bias, and A/B Test Forensics)

**Goal.** This lab investigates how the **Data Generating Process (DGP)** and **sampling decisions** can systematically distort machine learning conclusions—sometimes more than the model choice itself. Using the Titanic dataset as a controlled setting, I reproduced sampling-induced instability, corrected distribution shift with principled sampling, and implemented an A/B testing audit to detect instrumentation/assignment failures.

### Key Concepts
- **Sampling error / variance** from small or unlucky samples (even under “random” sampling)
- **Covariate shift** caused by mismatched feature/class distributions between train/test samples
- **Sample Ratio Mismatch (SRM)** as an A/B test red flag indicating pipeline or assignment issues

### Tech Stack
- **Python**, **pandas**, **numpy**
- **scipy** (`chi2_contingency` for Chi-Square tests)
- **scikit-learn** (stratified sampling utilities)

### What I Built / Did
1. **Manual Simple Random Sampling (SRS) on Titanic**
   - Simulated repeated SRS draws to show that “random” does not mean “stable.”
   - Demonstrated how small samples can yield **high variance** estimates (sampling error), changing observed survival rates and feature distributions across runs.

2. **Stratified Sampling to Remove Distribution Mismatch**
   - Implemented **stratified sampling** with `sklearn` to preserve key group/class proportions.
   - Reduced/removed **covariate shift** in class distributions introduced by naïve sampling, producing more consistent comparisons and model evaluation.

3. **SRM Forensic Audit for A/B Tests (Chi-Square)**
   - Built an SRM check that compares observed vs expected assignment counts.
   - Used **Chi-Square tests** to detect statistically significant mismatches that often indicate:
     - bucketing/assignment bugs,
     - logging dropouts,
     - filtering errors,
     - broken randomization,
     - or eligibility misconfiguration.

### Why It Matters
This lab emphasizes that bias is often “architected” upstream:
- If sampling changes your distributions, your model can look better/worse for reasons unrelated to learning.
- If A/B assignment is broken (SRM), the experiment can be invalid even if metrics look convincing.
- Understanding the DGP is part of doing ML responsibly.

---

## Theory: Unicorn-Only Datasets, Survivorship Bias, and Heckman “Ghost Data”

### Why “only successful TechCrunch Unicorns” creates Survivorship Bias
If you analyze only startups that became **Unicorns**, your dataset is **selected on the outcome** (or a close proxy for it). That means:
- Your sample is **not representative** of all startups.
- You systematically exclude the “non-survivors” (failed, stalled, acquired early, shut down, never funded, never covered).
- Any correlations you find (e.g., “Unicorns raise more early,” “Unicorns hire faster,” “Unicorns are in SF/AI”) are biased because those patterns are observed **conditional on surviving** long enough and succeeding enough to appear in TechCrunch / Unicorn lists.

In selection-bias terms, you observe `Y` (performance) **only when** `S=1` (selected into the dataset), and selection is strongly related to `Y`.

### What specific “Ghost Data” you’d need for a Heckman Correction
A Heckman correction addresses **sample selection bias** by modeling:
1) a **selection equation** (who gets into your dataset) and  
2) an **outcome equation** (startup performance)  
and then correcting for the non-random selection using the **Inverse Mills Ratio**.

To do that, you need the missing counterfactual population—the “ghosts” you don’t see when you only look at Unicorns:

**The Ghost Data you need:**  
A dataset of **non-Unicorn startups** (including failures and non-covered firms) with at least:
- **Selection indicator** `S`: whether the startup appears in TechCrunch / is included in your observation pipeline (1 = observed, 0 = not observed)
- **Outcome variable** `Y` (or a proxy): e.g., revenue growth, survival duration, exit value, follow-on funding, or a binary “failed vs alive” outcome
- **Pre-selection covariates** `X`: features determined *before* selection (founding year, industry, geography, founder background, early traction metrics if available)

**Crucial requirement (exclusion restriction):**  
You also need at least one variable `Z` that affects *selection into TechCrunch / visibility* but does **not directly affect true startup performance** after controlling for `X`. This is what makes the Heckman correction identifiable.

Examples of plausible `Z` (depends on your setting; you must justify it):
- PR/marketing spend or presence of a dedicated PR agency early on (visibility → coverage)
- Founder “media network” / prior press connections (coverage likelihood)
- Participation in high-visibility accelerators *as a coverage channel* (if it primarily affects press inclusion rather than fundamental performance, conditional on X)
- Timing/beat effects: whether TechCrunch had an active reporter covering that niche/location at the time (coverage propensity)

**In short:** to fix survivorship bias from Unicorn-only TechCrunch data with a Heckman correction, you need **the unseen population of non-Unicorn startups** (the “ghosts”) plus a defensible **selection driver** `Z` that shifts coverage/observability without directly shifting outcomes.

---

### Repo Structure (suggested)
- `notebooks/` — simulations + analysis
- `src/` — sampling utilities + SRM tests
- `data/` — dataset references (or instructions to download)
- `README.md` — this overview

### Reproducibility
1. Create environment (conda/venv)
2. Install deps: `pandas numpy scipy scikit-learn`
3. Run notebook(s) in order:
   - SRS simulation
   - stratified sampling experiment
   - SRM chi-square audit
