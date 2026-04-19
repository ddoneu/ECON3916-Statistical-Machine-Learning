# Clustering World Economies with K-Means & PCA

## Objective

Apply unsupervised learning to World Bank development indicators to discover whether data-driven clustering recovers established income group classifications without using income thresholds as an input.

## Methodology

- Collected 10 World Bank WDI indicators (7 retained after filtering sparse series) covering income, health, environment, connectivity, trade, labor, and urbanization for 251 countries via the `wbgapi` package
- Standardized all features with StandardScaler to ensure equal contribution to Euclidean distance calculations
- Fit K-Means clustering with K=4 to match the World Bank's four-tier income classification, using k-means++ initialization and a fixed random seed for reproducibility
- Projected the 7-dimensional feature space to 2D via PCA for visualization, capturing 64.5% of total variance
- Evaluated K=2 through K=10 using the elbow method (WCSS) and silhouette analysis to identify the data-driven optimal K
- Cross-tabulated algorithmic clusters against official World Bank income groups using a confusion-matrix heatmap
- Conducted a peer debate comparing K=3 vs K=5 on interpretability, labeling, and policy usefulness
- Applied the full pipeline (standardize, elbow/silhouette, K-Means, PCA) to 20,640 California Housing census tracts as a take-home extension

## Key Findings

- Silhouette analysis identified K=3 as the statistical optimum (score 0.322), though K=4 (0.273) was used to align with the World Bank's four income tiers
- At K=4, Cluster 3 mapped almost perfectly to the High income group (43/43 countries), while Cluster 2 captured Low and Lower-Middle income countries together (20 Low + 27 Lower-Middle), demonstrating that multivariate clustering largely recovers GDP-based classifications without ever seeing income thresholds
- Mismatches in Cluster 1 (30 High + 44 Upper-Middle) revealed that many upper-middle-income economies share development profiles with high-income countries when measured across health, trade, and connectivity dimensions, exposing the limitations of a GDP-only classification
- California Housing clustering (K=2, silhouette-optimal) split tracts geographically into Southern and Northern California rather than by economic characteristics, illustrating that geographic features can dominate clustering when included alongside economic variables
