# The Polynomial Trap: Interactive Bias-Variance Dashboard

## Objective
Built an interactive Streamlit dashboard to visualize the bias-variance tradeoff in polynomial regression, allowing users to explore how model complexity, noise, and training sample size affect fit quality and generalization performance in real time.

## Methodology
- Generated synthetic data from the nonlinear data-generating process \( y = \sin(2\pi x) + \varepsilon \), where noise was controlled by an adjustable standard deviation parameter.
- Implemented polynomial regression models of degree 1 through 15 using `PolynomialFeatures` and `LinearRegression` within a Scikit-learn pipeline.
- Added interactive Streamlit controls for:
  - polynomial degree
  - noise level (\(\sigma\))
  - number of training observations
- Visualized the selected polynomial fit against both the noisy training data and the true sine-wave function.
- Computed and displayed a live complexity curve comparing training RMSE and test RMSE across all polynomial degrees.
- Estimated the bias-variance decomposition through repeated sampling, tracking:
  - Bias\(^2\)
  - Variance
  - Mean Squared Error (MSE)
- Verified the decomposition numerically using the approximation:
  \[
  \text{MSE} \approx \text{Bias}^2 + \text{Variance} + \sigma^2
  \]
- Improved dashboard responsiveness by caching the repeated-sampling decomposition and making the most computationally expensive panel optional.

## Key Findings
- Low-degree models systematically underfit the nonlinear signal, producing high bias and weak predictive accuracy.
- As model complexity increased, bias declined, but variance rose as the fitted curve became more sensitive to sample noise.
- Test RMSE followed the expected U-shaped complexity pattern, with the best generalization performance occurring at intermediate polynomial degrees rather than at the most flexible models.
- Higher noise levels and smaller training samples amplified variance and made overfitting more severe.
- The repeated-sampling decomposition confirmed the central intuition of the lab: prediction error is minimized at the point where the bias-variance tradeoff is best balanced.
- The dashboard translated the notebook’s static analysis into an interactive diagnostic tool, making the mechanics of underfitting, overfitting, and model selection more transparent.
