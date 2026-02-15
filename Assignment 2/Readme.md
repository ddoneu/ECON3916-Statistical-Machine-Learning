# Audit 02: Deconstructing Statistical Lies

## Overview
This audit shows how “perfect” metrics can mislead decision-makers. Using **manual calculations** (MAD, Bayes, Chi-Square) and **simulations**, we audit three common failure modes in vendor pitches, AI tools, and experiment dashboards—plus an AI-assisted expansion on survivorship bias.

**Course:** ECON3916 — Module 2: Probability, Robustness, & Sampling  
**Role:** Data Quality Auditor (Pareto Ventures)

---

## Findings

### Finding 1: Latency Skew — The Mean Is a Vanity Metric
**The claim:** NebulaCloud advertises a mean latency of ~35ms.  
**The reality:** In heavy-tailed systems, averages hide pain.

**Simulation (1,000 requests):**
- 980 “normal” requests: `np.random.randint(20, 50, 980)` → **20–49ms**
- 20 “spike” requests: `np.random.randint(1000, 5000, 20)` → **1000–4999ms**
- Combined into `latency_logs`

**What we saw:**
- The **median** stays in the “normal” range (typical user looks fine).
- But **tail latency** (e.g., P99) jumps into **multi-second delays**, meaning a small % of users experience extreme slowdowns.

**Robustness check (SD vs MAD):**
- **Standard Deviation (SD)** inflates into the **hundreds of ms** because squaring amplifies outliers.
- **MAD (Median Absolute Deviation)** stays in **single-digit / low-teens ms** because it’s median-based and ignores extreme tails.

**Takeaway:** Negotiate SLAs using **P95/P99**, not the mean. Robust statistics (MAD/median) reveal what averages hide.

---

### Finding 2: False Positives — When “98% Accurate” Misleads
**The claim:** IntegrityAI has **98% sensitivity** and **98% specificity**.  
**The reality:** Accuracy is meaningless without the **base rate**.

We compute the posterior:
\[
P(\text{Cheater} \mid \text{Flagged}) =
\frac{\text{sensitivity}\cdot prior}{\text{sensitivity}\cdot prior + (1-\text{specificity})\cdot (1-prior)}
\]

**Results:**

| Scenario | Base Rate | P(Cheater \| Flagged) |
|---|---:|---:|
| Bootcamp | 50% | ~98% |
| Econ Class | 5% | ~72% |
| Honors Seminar | 0.1% | ~4.7% |

In the **Honors Seminar**, most flags are false positives: a 2% false-positive rate across ~999 clean students produces ~20 false flags versus ~1 true positive.

**Takeaway:** Even “high-accuracy” AI can generate mostly false accusations when prevalence is low. Always audit **base rates**.

---

### Finding 3: A/B Test Integrity — Sample Ratio Mismatch (SRM)
**The pitch:** FinFlash ran a “50/50” A/B test with 100,000 users and claims a huge win.  
**The data:** Control = 50,250 vs Treatment = 49,750.

We perform a **manual Chi-Square Goodness-of-Fit** test:

- Expected: [50,000, 50,000]
- Observed: [50,250, 49,750]
- \[
\chi^2 = \sum \frac{(O-E)^2}{E}
\]
- Result: **χ² = 2.5**
- Threshold at α = 0.05 (df=1): **3.84**

Since **2.5 < 3.84**, we do **not** reject a 50/50 split at the 5% level.

**Takeaway:** Before trusting uplift, verify assignment integrity. SRM can invalidate experiments—here, the mismatch is not statistically significant at 5%.

---

### Finding 4 (Expansion): Survivorship Bias — The Memecoin Graveyard
**The claim:** Crypto platforms highlight top-performing tokens, implying the market is profitable.  
**The reality:** Winners are shown; failures are hidden.

**Simulation (10,000 token launches):**
- Peak market caps drawn from a **Pareto (power-law) distribution** (heavy tail).
- Two datasets:
  - `df_all`: the full “graveyard”
  - `df_survivors`: top 1% only

**What we saw:**
- Most tokens peak near zero.
- The mean market cap of **survivors** can be **tens of times larger** than the mean of the full dataset (a survivorship multiplier that makes the market look far healthier than it is).

A dual-histogram plot is saved as:
- `survivorship_bias.png`

**Takeaway:** Studying only winners is a statistical lie. Always ask: **“Where are the failures?”**

---

## Tools & Methods
- **NumPy:** simulation + vectorized calculations  
- **Pandas:** DataFrames + filtering survivors  
- **Matplotlib:** visualization (dual histograms)  
- **Manual implementations:** MAD, Bayes’ Theorem posterior, Chi-Square GOF

---

## How to Reproduce
1. Open the Colab notebook.
2. Run all cells (seed set for reproducibility).
3. Outputs include:
   - SD vs MAD for latency logs
   - Posterior probabilities across base-rate scenarios
   - Chi-square SRM statistic + validity decision
   - Survivorship bias means + saved plot (`survivorship_bias.png`)

---

## Key Lesson
Numbers don’t lie—**but people choose which numbers to show**. Robustness, base rates, experiment integrity checks, and complete datasets are the antidote to statistical deception.
