 # Marketing Campaign Performance Analysis
 
> A comprehensive multi-dashboard Power BI analysis covering **Campaign Performance**, **Profitability**, and **Efficiency** across six digital marketing channels for fiscal year 2021, supported by Python-based Exploratory Data Analysis (EDA) on **69,669 campaign records**.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Power BI Overview](#power-bi-dashboard-overview)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Key Metrics & Insights](#key-metrics--insights)
- [Tech Stack & Tools Used](#tech-stack--tools-used)
- [Recommendations](#recommendations)
- [Documentation](#documentation)
- [Repository Structure](#repository-structure)

---

## Project Overview

This project presents a comprehensive analysis of marketing campaign performance using Python for data preprocessing and Power BI for interactive visualization. The objective is to evaluate campaign effectiveness, efficiency, and revenue outcomes across multiple channels and customer segments. The dashboard enables stakeholders to explore key marketing metrics, identify performance trends, and support data-driven decision-making. The analysis covers **69,669 campaign records** across **6 digital channels** and **5 campaign types** during the full 2021 fiscal year. Designed to support data-driven decisions at the executive and operational levels.

### Business Objective

The objective of this project is to:

- Analyse campaign performance across marketing channels
- Evaluate efficiency using CTR, Conversion Rate, and Customer Acquisition Cost (CAC)
- Assess revenue generation and campaign scalability
- Identify trends and patterns in customer engagement and conversions

**Business Questions**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

These objectives informed the key business questions that guided the analysis:

- Which campaigns deliver the strongest overall performance in terms of revenue and conversions?
- How does campaign performance evolve over time?
- Which campaigns are the most cost-efficient, and which are underperforming based on CAC and conversion outcomes?
- How does engagement (CTR) change as campaign reach increases?
- Which marketing channels contribute most to revenue and conversions, and how do they compare in efficiency?

**Project Architecture**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
```
Raw Dataset
      ↓
Python (Data Cleaning & Feature Engineering)
      ↓
Clean Analytical Dataset (CSV Export)
      ↓
Power BI (Data Model + DAX Measures)
      ↓
Interactive Multi-Page Dashboard
``` 

### Scope

| Attribute | Detail |
|-----------|--------|
| **Dataset** | `marketing_campaign_dataset.csv` — 69,669 records, 16 columns |
| **Reporting Period** | January 1, 2021 – December 31, 2021 |
| **Channels Covered** | Email, Facebook, Google Ads, Instagram, Website, YouTube |
| **Campaign Types** | Display, Social Media, Influencer, Search, Email |
| **Customer Segments** | Fashionistas, Foodies, Health & Wellness, Outdoor Adventurers |
| **Dashboards** | Campaign Performance, Profitability Analysis, Campaign Efficiency |

---

## Power BI Dashboard Overview


After importing the cleaned dataset from Google Colab into Power BI, I developed an interactive dashboard to comprehensively analyse campaign performance. To support this analysis, I created several DAX measures and calculated columns, including:

- Date Table
- Customer Acquisition Cost (CAC)
- Click-Through Rate (CTR)
- Return on Investment (ROI)
- ROAS Category
- ROAS Sort


### Dashboard Screenshots

### 1. Marketing Campaign Performance Analysis
> Cross-validates engagement and cost metrics to assess campaign-level efficiency and audience saturation.

<img src="https://github.com/FortunatusAdunola/Marketing-Campaign-Performance-Analysis/blob/main/Marketing%20Campaign%20Performance/Screenshots/dashboard_campaign_performance.png" width="800">

---

### 2. Marketing Campaign Profitability Analysis
> Evaluates ROAS distribution, revenue by campaign, and financial performance by customer segment.

<img src="https://github.com/FortunatusAdunola/Marketing-Campaign-Performance-Analysis/blob/main/Marketing%20Campaign%20Performance/Screenshots/dashboard_profitability.png" width="800">

---

### 3. Marketing Campaign Efficiency Analysis
> Tracks audience engagement, Customer Acquisition Cost (CAC), and Click-Through Rate (CTR) across campaign types and channels.

<img src="https://github.com/FortunatusAdunola/Marketing-Campaign-Performance-Analysis/blob/main/Marketing%20Campaign%20Performance/Screenshots/dashboard_efficiency.png" width="800">

---

>  **Note:** To view live interactive dashboards, open the `.pbix` file in **Power BI Desktop** or publish to **Power BI Service**.

---

## Exploratory Data Analysis (EDA)

All EDA was performed in Python using Pandas, NumPy, Matplotlib, and Seaborn.
📓 Full notebook: [`Marketing_Performance_Campaign.ipynb`](https://github.com/FortunatusAdunola/Marketing-Campaign-Performance-Analysis/blob/main/Marketing%20Campaign%20Performance/NoteBook/Marketing_Performance_Campaign.ipynb)

---

### 1. Import Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

---

### 2. Load Dataset

```python
df = pd.read_csv("marketing_campaign_dataset.csv")
df.head()
```

**Sample Records:**

| Campaign_ID | Company | Campaign_Type | Channel_Used | Conversion_Rate | Acquisition_Cost | ROI | Clicks | Impressions | Customer_Segment | Date |
|-------------|---------------------|---------------|--------------|-----------------|-----------------|-----|--------|-------------|------------------|------------|
| 1 | Innovate Industries | Email | Google Ads | 0.04 | $16,174.00 | 6.29 | 506.0 | 1922.0 | Health & Wellness | 2021-01-01 |
| 2 | NexGen Systems | Email | Google Ads | 0.12 | $11,566.00 | 5.61 | 116.0 | 7523.0 | Fashionistas | 2021-01-02 |
| 3 | Alpha Innovations | Influencer | YouTube | 0.07 | $10,200.00 | 7.18 | 584.0 | 7698.0 | Outdoor Adventurers | 2021-01-03 |
| 4 | DataTech Solutions | Display | YouTube | 0.11 | $12,724.00 | 5.55 | 217.0 | 1820.0 | Health & Wellness | 2021-01-04 |
| 5 | NexGen Systems | Email | YouTube | 0.05 | $16,452.00 | 6.50 | 379.0 | 4201.0 | Health & Wellness | 2021-01-05 |

**Initial dataset shape: 69,669 rows × 16 columns**

---

### 3. Data Preprocessing & Cleaning

#### 3.1 Data Type Correction

`Acquisition_Cost` was stored as a string with `$` symbols and commas. `Date` was stored as a plain object. Both were corrected:

```python
# Fix Acquisition_Cost: remove $ and commas, cast to float
df['Acquisition_Cost'] = df['Acquisition_Cost'].replace('[\$,]', '', regex=True).astype(float)

# Fix Date: parse to datetime
df['Date'] = pd.to_datetime(df['Date'])

# Extract Month for time-series grouping
df['Month'] = df['Date'].dt.month
```

**Before → After `df.info()` comparison:**

| Column | Before | After |
|--------|--------|-------|
| `Acquisition_Cost` | `object` | `float64` |
| `Date` | `object` | `datetime64[ns]`  |

**Dataset after type correction: 69,669 rows × 17 columns** (Month column added)

---

#### 3.2 Missing Values Check

```python
df.isnull().sum()
```

| Column | Missing Values |
|--------|---------------|
| Campaign_ID | 0 |
| Company | 0 |
| Campaign_Type | 0 |
| Channel_Used | 0 |
| Conversion_Rate | 0 |
| Acquisition_Cost | 0 |
| ROI | 0 |
| Location | **1** |
| Language | **1** |
| Clicks | **1** |
| Impressions | **1** |
| Engagement_Score | **1** |
| Customer_Segment | **1** |
| Date | **1** |

>  Only 1 missing record across 7 columns — all from the same row. At 69,669 total records, the missing rate is **< 0.002%** and has no material impact on analysis.

---

#### 3.3 Duplicate Check

```python
df.duplicated().sum()
# Output: 0
```

> No duplicate records found.

---

### 4. Descriptive Statistics

```python
df.describe()
```

| Metric | Conversion_Rate | Acquisition_Cost ($) | ROI | Clicks | Impressions | Engagement_Score |
|--------|----------------|---------------------|-----|--------|-------------|-----------------|
| **Count** | 69,669 | 69,669 | 69,669 | 69,668 | 69,668 | 69,668 |
| **Mean** | 0.080 | 12,507.37 | 5.01 | 549.33 | 5,504.09 | 5.49 |
| **Min** | 0.010 | 5,000.00 | 2.00 | 100.0 | 1,000.0 | 1.0 |
| **25th %ile** | 0.040 | 8,759.00 | 3.51 | 324.0 | 3,272.0 | 3.0 |
| **Median** | 0.080 | 12,500.00 | 5.01 | 549.0 | 5,514.0 | 5.0 |
| **75th %ile** | 0.120 | 16,250.00 | 6.51 | 775.0 | 7,747.0 | 8.0 |
| **Max** | 0.150 | 20,000.00 | 8.00 | 1,000.0 | 10,000.0 | 10.0 |
| **Std Dev** | 0.041 | 4,338.99 | 1.74 | 259.95 | 2,593.46 | 2.87 |

>  **Key Observations:**
> - Average acquisition cost is **$12,507** with a wide range of $5,000–$20,000
> - ROI ranges from **2.0x** (worst) to **8.0x** (best), averaging **5.0x**
> - Clicks and Impressions are uniformly distributed (mean ≈ median)
> - Engagement Score averages **5.49 / 10** with high variance (std = 2.87)

---

### 5. KPI Engineering

Derived metrics were calculated from existing columns to power the Power BI dashboards:

```python
# Click-Through Rate
df["CTR"] = df["Clicks"] / df["Impressions"]

# Number of Conversions
df["Conversions"] = df["Clicks"] * df["Conversion_Rate"]

# Revenue
df["Revenue"] = df["Acquisition_Cost"] * (1 + df["ROI"])

# Return on Ad Spend
df["ROAS"] = df["Revenue"] / df["Acquisition_Cost"]
```

**Sample KPI output (first 5 rows):**

| Campaign_ID | CTR | Conversions | Revenue ($) | ROAS |
|-------------|-----|-------------|-------------|------|
| 1 | 0.2633 | 20.24 | 117,908.46 | 7.29 |
| 2 | 0.0154 | 13.92 | 76,451.26 | 6.61 |
| 3 | 0.0759 | 40.88 | 83,436.00 | 8.18 |
| 4 | 0.1192 | 23.87 | 83,342.20 | 6.55 |
| 5 | 0.0902 | 18.95 | 123,390.00 | 7.50 |

---

### 6. EDA Results

#### 6.1 Channel Performance — Revenue vs. Acquisition Cost

```python
df.groupby("Channel_Used")[["Revenue", "Acquisition_Cost"]].sum()
```

| Channel | Total Revenue ($) | Total Acquisition Cost ($) |
|---------|------------------|--------------------------|
| Email | 881,924,000 | 146,363,556 |
| Instagram | 875,563,100 | 146,458,557 |
| Website | 873,629,300 | 145,508,409 |
| Facebook | 864,218,100 | 143,656,950 |
| Google Ads | 869,699,700 | 144,713,061 |
| YouTube | 868,983,200 | 144,675,150 |

>  All six channels generate nearly identical revenue (~$864M–$882M), confirming a well-balanced portfolio with no single dominant channel.

---

#### 6.2 Average ROI by Channel

```python
df.groupby("Channel_Used")["ROI"].mean().sort_values(ascending=False)
```

| Rank | Channel | Avg ROI |
|------|---------|---------|
|  1 | Email | 5.026 |
|  2 | Google Ads | 5.014 |
|  3 | Website | 5.012 |
| 4 | Facebook | 5.007 |
| 5 | YouTube | 5.003 |
| 6 | Instagram | 4.978 |

>  Email leads ROI at 5.03x while Instagram trails at 4.98x. The spread of only **0.048 points** across all channels signals near-uniform efficiency — strong execution across the board with limited channel differentiation.

---

#### 6.3 Conversion Rate by Channel

```python
df.groupby("Channel_Used")["Conversion_Rate"].mean()
```

| Channel | Avg Conversion Rate |
|---------|-------------------|
| Google Ads | 8.05% |
| Website | 8.03% |
| Email | 8.02% |
| Facebook | 7.99% |
| Instagram | 7.99% |
| YouTube | 7.98% |

>  Conversion rates are virtually identical across all channels (~8%). Google Ads leads marginally at 8.05%, reinforcing its position as a reliable performance channel.

---

#### 6.4 Top 10 Performing Campaigns (by ROI = 8.0x)

```python
df.sort_values("ROI", ascending=False).head(10)
```

| Campaign_ID | Company | Type | Channel | Acq. Cost ($) | ROI | Revenue ($) |
|-------------|---------|------|---------|--------------|-----|-------------|
| 29550 | NexGen Systems | Display | Facebook | 6,854 | 8.0 | 61,686 |
| 3232 | Alpha Innovations | Display | Instagram | 12,759 | 8.0 | 114,831 |
| 20016 | Innovate Industries | Influencer | Email | 12,130 | 8.0 | 109,170 |
| 29082 | Innovate Industries | Email | Facebook | 9,611 | 8.0 | 86,499 |
| 16538 | Innovate Industries | Email | YouTube | 12,085 | 8.0 | 108,765 |
| 47113 | DataTech Solutions | Influencer | Google Ads | 17,969 | 8.0 | 161,721 |
| 44576 | TechCorp | Display | Email | 12,845 | 8.0 | 115,605 |
| 67607 | Innovate Industries | Influencer | Facebook | 11,172 | 8.0 | 100,548 |
| 64344 | Innovate Industries | Influencer | YouTube | 12,094 | 8.0 | 108,846 |
| 15652 | DataTech Solutions | Display | Website | 12,708 | 8.0 | 114,372 |

>  Top performers span multiple channels and campaign types — no single format dominates peak ROI. **Innovate Industries** appears 5 times, making it the strongest-performing company in the portfolio.

---

#### 6.5 Bottom 10 Performing Campaigns (by ROI = 2.0x)

```python
df.sort_values("ROI").head(10)
```

| Campaign_ID | Company | Type | Channel | Acq. Cost ($) | ROI | Revenue ($) |
|-------------|---------|------|---------|--------------|-----|-------------|
| 57915 | TechCorp | Search | YouTube | 19,404 | 2.0 | 58,212 |
| 32616 | TechCorp | Email | Website | 7,038 | 2.0 | 21,114 |
| 68623 | NexGen Systems | Display | Instagram | 11,071 | 2.0 | 33,213 |
| 57348 | TechCorp | Search | Email | 7,562 | 2.0 | 22,686 |
| 40005 | TechCorp | Influencer | Facebook | 19,356 | 2.0 | 58,068 |
| 15477 | NexGen Systems | Influencer | Email | 6,340 | 2.0 | 19,020 |
| 60545 | TechCorp | Influencer | Facebook | 18,896 | 2.0 | 56,688 |
| 23052 | Innovate Industries | Email | Facebook | 9,003 | 2.0 | 27,009 |
| 10286 | Alpha Innovations | Email | Facebook | 18,589 | 2.0 | 55,767 |
| 7824 | TechCorp | Social Media | Google Ads | 17,374 | 2.0 | 52,122 |

>  **TechCorp dominates the worst-performing list** with 5 of 10 entries. High acquisition costs ($17K–$19K) paired with the minimum ROI of 2.0x indicate severely inefficient spend. These campaigns are prime candidates for immediate budget reallocation.

---

### 7. Export Cleaned Dataset

```python
df.to_csv("marketing_campaign_cleaned.csv", index=False)
```

> The cleaned dataset includes all engineered KPIs (`CTR`, `Conversions`, `Revenue`, `ROAS`, `Month`) and corrected data types — ready for Power BI ingestion or further modelling.

---

## Key Metrics & Insights

### Global KPIs (Power BI Dashboards)

| Metric | Value |
|--------|-------|
| **Total Ads Spent** | $2.4 Billion |
| **Total Revenue** | $14.5 Billion |
| **Sum of Conversions** | 8.52 Million |
| **Average ROAS** | 6.00x |
| **Average ROI** | 5.00x |
| **Average CTR** | 14.04% |

### Top Findings

| # | Finding |
|---|---------|
| 01 | Every $1 invested returned $6 in gross revenue — well above the 2–4x industry ROAS benchmark |
| 02 | Campaign `170866` has a CAC ~25% above the next most expensive campaign |
| 03 | CTR drops from 60%+ at low impressions to under 15% beyond 5,000 impressions — audience saturation |
| 04 | All campaign types delivered CTR within a 0.19% band — strong execution parity |
| 05 | **TechCorp** accounts for 5 of 10 worst-performing campaigns by ROI |
| 06 | **Innovate Industries** accounts for 5 of 10 top-performing campaigns by ROI |
| 07 | All four customer segments generate near-equal revenue ($2.88bn–$2.93bn) |

---

## Tech Stack & Tools Used

| Layer | Tool / Technology | Purpose |
|-------|------------------|---------|
| **EDA & Analysis** | Python 3 (Pandas, NumPy) | Data cleaning, KPI engineering, statistical analysis |
| **Visualization (EDA)** | Matplotlib, Seaborn | Exploratory charts and distribution plots |
| **BI Dashboards** | Microsoft Power BI Desktop | Interactive dashboard design and reporting |
| **Data Modeling** | Power Query (M Language) | Data transformation and shaping |
| **DAX** | Data Analysis Expressions | KPI calculations — ROAS, ROI, CTR |
| **Data Source** | CSV (69,669 records) | Raw campaign and conversion data |
| **Documentation** | Microsoft Word (.docx) | Stakeholder report generation |
| **Notebook** | Jupyter Notebook (.ipynb) | Python EDA environment |
| **Version Control** | GitHub | Project repository and collaboration |

---

## Recommendations

| # | Priority | Recommendation |
|---|----------|----------------|
| 1 | 🔴 **Critical** | **Audit Campaign 170866** — Assess revenue contribution and LTV. If CAC is not justified, reallocate budget to lower-CAC campaigns. |
| 2 | 🔴 **Critical** | **Review TechCorp campaigns** — 5 of 10 worst-performing campaigns are TechCorp. Conduct a full strategic review of their targeting, creative, and channel mix. |
| 3 | 🟠 **High** | **Implement impression frequency caps** — Set a 3,000–5,000 impression ceiling per segment and rotate creatives every 2–3 weeks to counter audience fatigue. |
| 4 | 🟠 **High** | **Conduct Customer LTV segmentation analysis** — Revenue parity across segments may mask LTV differences; redirect budgets based on lifetime profitability. |
| 5 | 🟡   **Medium** | **Sunset Low ROAS campaigns** — Establish a 3x ROAS minimum threshold and reallocate budgets from underperformers each quarter. |
| 6 | 🟡   **Medium** | **Test channel concentration** — Pilot a 20% budget increase toward Email and Google Ads (highest ROI) over 90 days to measure marginal returns. |
| 7 | 🟡   **Medium**  | **Explore seasonal amplification** — Overlay revenue-per-quarter with demand calendars to identify high-value seasonal spend windows. |
| 8 | 🟢 **Low** | **Diversify campaign creative formats** — Test video-first and interactive ad formats to break the uniform CTR plateau. |

---

## Documentation

- 📄 [`Marketing_Analytics_Report.docx`](docs/Marketing_Analytics_Report.docx) — Full stakeholder Word report
- 📓 [`Marketing_Performance_Campaign.ipynb`](https://github.com/FortunatusAdunola/Marketing-Campaign-Performance-Analysis/blob/main/Marketing%20Campaign%20Performance/NoteBook/Marketing_Performance_Campaign.ipynb)— Python EDA notebook

---

## Repository Structure

```
marketing-analytics-powerbi/
│
├── dashboards/
│   └── Marketing_Campaign_Analysis.pbix        # Power BI source file
│
├── notebooks/
│   └── Marketing_Performance_Campaign.ipynb    # Python EDA notebook
│
├── screenshots/
│   ├── dashboard_campaign_performance.png
│   ├── dashboard_profitability.png
│   └── dashboard_efficiency.png
│
├── data/
│   ├── marketing_campaign_dataset.csv          # Raw source data
│   └── marketing_campaign_cleaned.csv          # Cleaned dataset with KPIs
│
├── docs/
│   └── Marketing_Analytics_Report.docx         # Full stakeholder report
│
└── README.md
```

---

## License

This project is intended for internal business and portfolio use. All data has been anonymized for public sharing.

---

*Dataset: 69,669 records | Reporting Period: January 1 – December 31, 2021 | Prepared for internal stakeholder review*
