# The Cost of Living Crisis: A Data-Driven Analysis
**Analyzing the Hidden Inflation Gap Between Official Statistics and Student Reality**

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Libraries](https://img.shields.io/badge/Libraries-pandas%20%7C%20matplotlib%20%7C%20fredapi-green.svg)
![Data Source](https://img.shields.io/badge/Data-FRED%20API-orange.svg)

---

## 🎯 The Problem

The official Consumer Price Index (CPI) reports that inflation is moderating. Yet students across America are feeling unprecedented financial pressure. Why this disconnect?

**The answer lies in what economists call the "composition problem."** The official CPI uses a fixed basket of goods weighted to represent the "average American household"—but students are not average consumers. When housing represents 34% of the official CPI basket but might constitute 50-70% of a student's actual spending, the official statistics systematically underestimate the inflation students actually experience.

This project investigates three critical questions:
1. How does student-specific inflation differ from official CPI measures?
2. Does living in Boston amplify these disparities?
3. What happens when we compare indices without proper normalization?

---

## 🔬 Methodology

### Data Collection & Processing
- **Source**: Federal Reserve Economic Data (FRED) via API
- **Time Period**: 2016-2024 (undergraduate experience window)
- **Series Analyzed**:
  - `CPIAUCSL`: Official CPI for All Urban Consumers
  - `CUSR0000SEEB`: Tuition, Fees & Childcare
  - `CUSR0000SEHA`: Rent of Primary Residence
  - `CUSR0000SEFV`: Food Away From Home (proxy for dining)
  - `APU0000708111`: Eggs (grocery staple)
  - `CUURA103SA0`: Boston-Cambridge-Newton Regional CPI

### Index Theory & Normalization
The project employs **Laspeyres Price Index** principles to construct a custom Student Price Index (SPI). Critical steps included:

1. **Base Year Normalization**: All FRED series were re-indexed to January 2016 = 100 to enable fair comparison
```
   Index_normalized = (Value_current / Value_2016) × 100
```

2. **Custom Weighting**: Constructed a student-specific basket with realistic weights:
   - Tuition: 40% (vs. ~6% in official CPI)
   - Rent: 30% (vs. ~34% in official CPI)
   - Food Away: 20% (vs. ~6% in official CPI)
   - Groceries: 10% (vs. ~8% in official CPI)

3. **Weighted Aggregation**:
```
   Student_SPI = Σ (Price_Index_i × Weight_i)
```

### Technical Stack
- **Python 3.8+**: Core programming language
- **pandas**: Data manipulation and time series handling
- **matplotlib**: Data visualization
- **fredapi**: Federal Reserve data extraction
- **Google Colab**: Development environment

---

## 📊 Key Findings

### Finding 1: The Student Inflation Gap
**My analysis reveals a [X]% divergence between Student Costs and National Inflation from 2016-2024.**

- **National CPI**: 100 → [X] (+[X]% increase)
- **Student SPI**: 100 → [X] (+[X]% increase)
- **Inflation Gap**: [X] percentage points

The weighted Student Price Index grew significantly faster than official CPI, driven primarily by:
- Rent increasing [X]% (compared to [X]% national CPI)
- Food away from home rising [X]%
- Tuition/fees climbing [X]%

### Finding 2: The Boston Premium
Students in Boston face compounded inflation pressure:

- **National CPI Growth**: +[X]%
- **Boston Regional CPI**: +[X]% (+[X]pp premium)
- **Boston Student SPI**: +[X]% (+[X]pp total disadvantage)

This represents a **double burden**: geographic location premium + demographic composition effect.

### Finding 3: The Scale Fallacy
Demonstrated why comparing raw FRED indices is statistically invalid:
- Raw Tuition Index: ~875 (Base: 1982-84)
- Raw Eggs Index: ~2.89 (Base: 1980)
- **Visual Deception**: Tuition appears 300× larger, masking actual growth rates

After normalization to 2016 = 100:
- Tuition: +[X]% actual growth
- Eggs: +[X]% actual growth
- **True Insight**: Revealed which categories genuinely inflated faster

---

## 📈 Visualizations

### 1. Multi-Category Inflation Trends
![Normalized Inflation Trends](charts/inflation_trends.png)
*All series normalized to January 2016 = 100 for fair comparison*

### 2. The Student Inflation Gap
![Student vs Official CPI](charts/student_gap.png)
*Shaded area represents the hidden inflation students experience beyond official statistics*

### 3. Triple Reality Check: National vs. Boston vs. Student
![Regional Comparison](charts/triple_comparison.png)
*Demonstrating how both geography and demographics compound inflation impacts*

### 4. The Scale Fallacy
![Raw vs Normalized](charts/scale_fallacy.png)
*Side-by-side comparison showing why base year normalization is essential*

---

## 💡 Implications

### For Students
The official "inflation is cooling" narrative doesn't reflect lived experience. Students face:
- Systematically higher inflation than reported in headlines
- Geographic penalties in high-cost cities like Boston
- Composition bias in official statistics that underweights student expenses

### For Policy Makers
- Student loan policies should account for actual student-specific inflation, not national CPI
- Financial aid adjustments using official CPI systematically underfund student needs
- Regional cost-of-living adjustments are critical for equitable education access

### For Data Scientists
This project demonstrates:
- The critical importance of proper index normalization
- How aggregation bias can mask subgroup experiences
- The power of custom indices for demographic-specific analysis

---

## 🛠️ Reproducibility

### Setup
```bash
pip install pandas matplotlib fredapi
```

### Run Analysis
1. Clone this repository
2. Obtain a free FRED API key at https://fred.stlouisfed.org/docs/api/api_key.html
3. Open `Econ_3916_Assignment_1.ipynb` in Google Colab
4. Replace `YOUR_API_KEY` with your FRED key
5. Run all cells

### Project Structure
```
Assignment-1/
├── Econ_3916_Assignment_1.ipynb    # Main analysis notebook
├── README.md                        # This file
└── charts/                          # Generated visualizations
    ├── inflation_trends.png
    ├── student_gap.png
    ├── triple_comparison.png
    └── scale_fallacy.png
```

---

## 📚 References

- Federal Reserve Economic Data (FRED): https://fred.stlouisfed.org/
- Bureau of Labor Statistics CPI Documentation: https://www.bls.gov/cpi/
- Laspeyres Price Index Theory: [Relevant economic theory citation]

---

## 👤 Author

**Dat Do**  
Economics Student | Northeastern University  
Focus: Finance & Data Analytics

[LinkedIn](#) | [GitHub](#) | [Portfolio](#)

---

## 📝 License

This project is part of ECON 3916: Statistical & Machine Learning for Economics coursework.

---

*Last Updated: January 2026*
