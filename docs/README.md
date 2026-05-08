# Customer Churn & Revenue Leakage Analysis
### Olist Brazilian E-Commerce Platform · 2016–2018

![Python](https://img.shields.io/badge/Python-3.11-blue) ![Power BI](https://img.shields.io/badge/PowerBI-Dashboard-yellow) ![SQL](https://img.shields.io/badge/SQL-Queries-lightblue) ![Dataset](https://img.shields.io/badge/Dataset-100K%20Orders-green)

---

## Business Problem

A Brazilian e-commerce platform is silently losing customers after their first purchase.
With no visibility into *who* is leaving, *why* they leave, or *how much revenue* is at risk,
the retention team has no data to act on.

**This project answers 3 questions the business needs answered:**
1. Which customers are at risk of churning — and how much revenue do they represent?
2. What is the primary driver of customer dissatisfaction?
3. Where should the retention budget be focused for maximum ROI?

---

## Key Findings

| # | Insight | Business Impact |
|---|---------|----------------|
| 1 | **40.1% of platform revenue (R$6.45M)** sits in At Risk or Lost segments | Immediate retention intervention needed |
| 2 | **Cohort retention collapses below 1% by Month 2** — across every acquisition cohort | Structural one-time buyer crisis, not seasonal |
| 3 | **Even Champions average only 1.11 purchases** — churn is a frequency problem, not a spend problem | Post-purchase re-engagement is the #1 lever |
| 4 | **Delivery delay >4 days → review score crashes from 4.03 to 2.10** | SLA enforcement = direct churn reduction |
| 5 | **22,975 Potential Loyalists represent R$3.7M recoverable revenue** | One targeted campaign could unlock this segment |

---

## Recommendations

**1. Launch a Potential Loyalist re-engagement campaign (Quick win)**
Target 22,975 recent buyers who haven't returned. Personalised "we think you'd love this"
email within 30 days of first purchase. Expected uplift: 5–8% conversion = R$185K–296K recovered.

**2. Enforce a 4-day delivery SLA in high-churn states (Structural fix)**
Review score drops from 4.03 → 2.10 beyond 4 days late. Partner with logistics providers
in SP and RJ (highest order volume) to enforce SLA. Direct impact on review score → repeat rate.

**3. Build a Champion loyalty programme (Protect revenue)**
15,441 Champions drive R$2.8M revenue at an avg R$180 spend. A points-based loyalty programme
with early access and VIP offers retains this segment before they slide into At Risk.

---

## Dashboard Preview

### Page 1 — Executive Summary
> 4 KPI cards · Customer segment bar chart · Revenue donut chart

### Page 2 — RFM Analysis
> Revenue by segment · Behavioural profile matrix · Recency vs Monetary scatter · Segment slicer

### Page 3 — Churn Drivers
> Monthly order trend · Delivery delay vs review score · Scatter: delay × payment × score

---

## Dataset

| Source | Olist Brazilian E-Commerce Public Dataset |
|--------|------------------------------------------|
| Link | [kaggle.com/olistbr/brazilian-ecommerce](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) |
| Size | 99,441 orders · 96,096 unique customers · 9 CSV files |
| Period | October 2016 – October 2018 |

---

## Tools & Tech Stack

| Tool | Purpose |
|------|---------|
| **Python** (pandas, seaborn, matplotlib, scikit-learn) | Data cleaning, EDA, RFM scoring, cohort analysis |
| **SQL** | Data merging, segmentation queries, aggregations |
| **Power BI** | 3-page interactive dashboard with DAX measures |
| **Excel** | Initial data audit and null profiling |

---

## Project Structure

```
churn-analysis/
│
├── data/
│   ├── raw/               ← 9 original Olist CSVs (read-only)
│   └── cleaned/           ← master_table.csv · rfm_table.csv
│
├── notebooks/
│   ├── 01_data_audit.ipynb
│   └── 02_eda_charts.ipynb
│
├── sql/                   ← SQL equivalents of all merges & aggregations
├── dashboard/             ← Churn_Analysis_Olist.pbix + PDF export
├── outputs/
│   ├── charts/            ← 4 analysis charts (PNG)
│   └── reports/           ← Executive summary PDF
│
└── docs/
    ├── README.md
    └── data_dictionary.md
```

---

## How to Run

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/churn-analysis-olist.git
cd churn-analysis-olist

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn plotly scikit-learn openpyxl

# 3. Download dataset
# Go to: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
# Place all 9 CSVs in data/raw/

# 4. Run notebooks in order
jupyter notebook notebooks/01_data_audit.ipynb
jupyter notebook notebooks/02_eda_charts.ipynb

# 5. Open dashboard
# Open dashboard/Churn_Analysis_Olist.pbix in Power BI Desktop
```

---

## Key Metrics Summary

```
Total Platform Revenue     : R$ 16,081,421
Revenue at Risk            : R$  6,455,428  (40.1%)
Recoverable Revenue        : R$  3,698,975
Unique Customers Analysed  :       96,096
At Risk Customers          :       22,966
Potential Loyalists        :       22,975
Champion Customers         :       15,441
Avg Delivery Delay         :      -11.88 days (mostly early)
Cohort Retention @ Month 2 :        <1% across all cohorts
```

---

## About

Built as part of a Data Analyst portfolio project to demonstrate end-to-end analytical skills:
data cleaning → EDA → RFM modelling → cohort analysis → business storytelling → dashboard.

**Connect:** [LinkedIn](https://linkedin.com/in/YOUR_PROFILE) · [GitHub](https://github.com/YOUR_USERNAME)
