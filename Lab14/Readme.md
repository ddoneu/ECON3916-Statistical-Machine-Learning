## AI Capex Diagnostic Modeling

**Objective:**  
Evaluated the reliability of an OLS model predicting AI software revenue using 2026 Nvidia AI capital expenditure and deployment data, with a focus on diagnosing structural specification failures and correcting inference using robust estimation techniques.

**Methodology:**  
- Built an initial Ordinary Least Squares (OLS) regression model to estimate the relationship between AI software revenue, capital expenditure, and deployment-related metrics.  
- Assessed model validity by diagnosing key structural issues, with emphasis on heteroscedasticity and multicollinearity.  
- Examined the residual structure to identify non-constant error variance across capital expenditure tiers, particularly at higher spending levels.  
- Evaluated how these violations distorted conventional inference, especially by producing overly narrow standard errors and misleadingly small p-values.  
- Re-estimated inference using **HC3 robust standard errors** to correct for heteroscedasticity and obtain more reliable significance testing.  
- Compared naive OLS results with heteroscedasticity-consistent estimates to distinguish apparent statistical significance from defensible empirical evidence.  

**Key Findings:**  
The analysis identified severe heteroscedasticity that intensified at higher levels of AI capital expenditure, indicating that model error variance expanded systematically across spending tiers. In the baseline OLS specification, this structural failure created false statistical confidence by understating standard errors and inflating the apparent significance of several predictors. After applying **HC3 robust standard errors**, the estimated uncertainty widened appropriately, showing that the true statistical significance of deployment-related metrics was materially weaker than suggested by the naive model. These results highlight the importance of robust inference when modeling high-variance capital deployment environments.
