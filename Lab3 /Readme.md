# Vietnam Economic Analysis Dashboard

This notebook analyzes Vietnam’s key economic indicators and benchmarks its development against:
- **Lower Middle Income (LMC)** average
- **World (WLD)** average

It retrieves data from the **World Bank Group API (wbgapi)**, processes and derives additional macro metrics, then visualizes trends using **matplotlib** and **seaborn**.

---

## Data Sources

All indicators are fetched via **World Bank Group API (wbgapi)** for:

- **VNM**: Vietnam  
- **LMC**: Lower Middle Income (average)  
- **WLD**: World (average)

---

## Key Indicators Analyzed

The notebook fetches and analyzes:

- **Real GDP per Capita** (constant 2015 US$)
- **Real GDP** (constant 2015 US$)
- **Inflation (CPI)**
- **Unemployment Rate**
- **Labor Force Participation Rate**
- **Total Labor Force**
- **Tax Revenue (% of GDP)**
- **Government Expenditure (% of GDP)**
- **Exports of Goods & Services (% of GDP)**
- **Imports of Goods & Services (% of GDP)**
- **Gross Domestic Savings (% of GDP)**
- **Gross Capital Formation (Investment, % of GDP)**

---

## Derived Metrics

To provide deeper insights, the notebook computes:

- **Natural Rate of Unemployment**  
  5-year moving average of the unemployment rate.

- **Productivity**  
  Real GDP divided by total labor force.

- **Net Capital Outflow (NCO)**  
  Exports minus imports.

- **Budget Balance**  
  Tax revenue minus government expenditure.

---

## Visualizations

The notebook generates the following charts:

### 1) Benchmarking Development (Real GDP per Capita)
Line plots comparing Vietnam vs. LMC and World averages.

### 2) Labor Market Dynamics
Dual-line plot:
- Labor Force Participation Rate
- Unemployment Rate

### 3) Monetary Stability
Bar chart of Vietnam’s inflation rate (CPI).

### 4) Capital Accumulation
Line plots comparing:
- Gross Domestic Savings
- Gross Capital Formation (Investment)

### 5) External Balance
- Fill-between plots for exports vs. imports  
- Line plot for Net Capital Outflow (Exports − Imports)

### 6) Fiscal Policy
- Line plots for tax revenue and government expenditure  
- Fill-between area showing the fiscal balance

### 7) Executive Dashboard (2×3)
A compact macro snapshot with six panels:
1. Real GDP (line)
2. Inflation (bar with zero baseline)
3. Unemployment (line)
4. Fiscal balance (tax vs. spending fill-between)
5. Trade balance (exports vs. imports fill-between)
6. Savings vs. investment (dual lines)

### 8) Chart Critique Examples
Includes intentionally bad examples (e.g., truncated bar chart, meaningless pie chart) to demonstrate common visualization pitfalls.

---

## Setup and Usage

### Install Dependencies
```bash
pip install wbgapi pandas matplotlib seaborn
