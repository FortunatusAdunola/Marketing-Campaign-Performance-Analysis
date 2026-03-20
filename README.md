**Marketing Campaign Performance Analysis**
------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Project Overview**
This project presents a comprehensive analysis of marketing campaign performance using Python for data preprocessing and Power BI for interactive visualization. The objective is to evaluate campaign effectiveness, efficiency, and revenue outcomes across multiple channels and customer segments. The dashboard enables stakeholders to explore key marketing metrics, identify performance trends, and support data-driven decision-making.

**Objectives**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

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
**Data Cleaning & Preparation**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

Data preprocessing was performed using Python to ensure accuracy and consistency:

- Checked and handled missing values
- Validated numerical fields (cost, revenue, conversions)
- Removed inconsistencies and ensured data integrity
- Verified calculated fields such as CTR and Conversion Rate
- Exported cleaned dataset for visualisation in Power BI
