# LAB2 The Illusion of Growth & the Composition Effect

A data-driven walkthrough of how *nominal* wage growth can hide decades of stagnation once inflation is accounted for—and how the apparent “wage boom” of 2020 was largely a statistical artifact driven by workforce composition changes.

---

## Objective
Build a reproducible Python pipeline that pulls live macroeconomic time series from the Federal Reserve Economic Data (FRED) API to:
- convert nominal wages into **real wages** (inflation-adjusted),
- identify the **2020 pandemic spike** in measured wages, and
- correct for **composition bias** using the **Employment Cost Index (ECI)** as a control.

---

## Methodology (API-first + bias correction)
- **Data ingestion (live FRED API):**
  - Pulled nominal wage series (FRED series ID: **AHETPI**).
  - Pulled price level data via CPI (commonly used CPI series on FRED) to deflate nominal wages into **Real Wages**.
- **Real wage construction:**
  - Converted nominal earnings into an inflation-adjusted wage index to remove the “money illusion” effect.
- **Anomaly detection (Pandemic Spike):**
  - Flagged the sharp 2020 increase in nominal/real wage measures as an outlier relative to the long-run trend.
- **Composition-effect control (ECI correction):**
  - Fetched the **Employment Cost Index (ECI)** to isolate underlying labor-cost dynamics.
  - Compared wage measures to ECI during 2020 to demonstrate the spike was driven by **who was working**, not a sudden broad-based jump in pay.

---

## Key Findings (The “Pandemic Paradox”)
- **Money Illusion:** Over ~50 years, nominal wages rise substantially, but **real wages are comparatively flat**, highlighting how inflation can mask stagnation.
- **Pandemic Paradox (2020):** The apparent wage surge in 2020 is largely explained by the **composition effect**—lower-wage workers disproportionately exited the labor force, mechanically raising the average of those still employed.
- **ECI as a reality check:** ECI provides a cleaner signal of labor-cost pressure and supports the conclusion that the 2020 “wage boom” was **not** a broad-based increase in labor demand-driven wage growth.

---

**Tech Stack:** Python · fredapi · pandas · matplotlib  
**Keywords:** Real Wages · CPI Deflation · Composition Effect · Employment Cost Index · FRED API
