# Lab 24 — Double Machine Learning on 401(k) Eligibility

## Overview
This repository contains my work for **ECON 3916: Statistical & Machine Learning for Economics**, Lab 24: **Causal ML — Double Machine Learning**. The goal of this lab is to estimate the causal effect of **401(k) eligibility** on **net total financial assets** using **Double Machine Learning (DML)**, and then study how that treatment effect varies across the income distribution through **CATE analysis**.

The lab also includes an **AI Expansion** section, where I created a dashboard-style summary and sensitivity-analysis interpretation to evaluate how robust the estimated treatment effect is to possible omitted-variable bias.

---

## Research Question
**How much does 401(k) eligibility increase household net financial assets, after controlling for confounding factors?**

A second question is whether the treatment effect differs across income quartiles, since higher-income households may be better positioned to contribute, capture employer matches, and benefit from tax deferral.

---

## Dataset
The analysis uses the **401(k) pension dataset** discussed in the lab, with:

- **Outcome:** `net_tfa` — net total financial assets  
- **Treatment:** `e401` — 401(k) eligibility  
- **Confounders:** age, income, family size, education, marital status, two-earner status, IRA participation, and homeownership

---

## Methods
### 1. Regularization Bias Demo
The guided section illustrates why naïve machine learning can be problematic for causal inference. In particular, regularized prediction methods such as LASSO may shrink the treatment coefficient and bias the estimated treatment effect toward zero.

### 2. Double Machine Learning (DML)
I used a **DoubleML Partially Linear Regression (PLR)** setup with:

- `DoubleMLData` for causal-data structure
- `RandomForestRegressor` for both nuisance functions
- `n_estimators = 500`
- `max_depth = 7`
- `random_state = 42`
- `n_folds = 5` for cross-fitting

DML works by first residualizing both the outcome and treatment with respect to the observed controls, then estimating the treatment effect using those residuals. This separates prediction from causal estimation and reduces bias from overfitting.

### 3. CATE by Income Quartile
To study treatment-effect heterogeneity, I split the sample into four income quartiles and estimated DML separately for each subgroup. This allows the effect of 401(k) eligibility to vary across the income distribution rather than assuming a single common effect for everyone.

### 4. AI Expansion: Sensitivity + Dashboard
For the AI Expansion, I created an interactive dashboard-style presentation of:

- the full-sample ATE
- quartile-level CATE estimates
- a sensitivity-analysis summary
- a policy interpretation of efficiency vs. equity trade-offs

I also documented the AI workflow and my manual verification steps in a separate verification log.

---

## Main Results

### Full-Sample ATE
The estimated **Average Treatment Effect (ATE)** is approximately **$8,751**, with a 95% confidence interval of about **[$6,038, $11,463]**. This suggests that, after accounting for confounding using DML, 401(k) eligibility has a positive and economically meaningful effect on household financial assets.

### CATE by Income Quartile
The treatment effect varies substantially by income group:

- **Q1:** about **$4,404**
- **Q2:** about **$3,397**
- **Q3:** about **$6,736**
- **Q4:** about **$18,900**

This pattern suggests that 401(k) eligibility benefits all income groups, but the gains are much larger for higher-income households. A likely explanation is that higher-income households have more contribution capacity, receive larger dollar-value employer matches, and benefit more from the tax advantages of retirement saving.

### Sensitivity Analysis
The sensitivity analysis indicates that the estimated treatment effect is **moderately robust** to omitted-variable bias. The robustness value implies that an unobserved confounder would need to explain a meaningful share of residual variation in both treatment assignment and financial assets to fully explain away the result.

---

## Economic Interpretation
The findings show that 401(k) eligibility appears to increase household wealth on average, but the size of the benefit is not evenly distributed. From an **efficiency** standpoint, targeting higher-income households generates the largest estimated gains in assets. From an **equity** standpoint, however, lower-income households may need complementary policies—such as automatic enrollment, matching contributions, or targeted subsidies—to translate eligibility into meaningful savings growth.

This is why CATE analysis matters: a single ATE can hide important policy-relevant heterogeneity across subgroups.

---

## Repository Structure
```text
econ-lab-24-double-ml/
├── README.md
├── notebooks/
│   └── lab_24_double_ml.ipynb
├── figures/
│   ├── regularization_bias.png
│   └── cate_by_quartile.png
└── verification-log.md
