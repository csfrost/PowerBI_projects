# 📊 Telecom Customer Churn Analysis

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success)
![License](https://img.shields.io/badge/License-MIT-green)

> **Interactive Power BI dashboards analyzing customer churn patterns, risk factors, and revenue impact for a telecommunications company.**

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Key Metrics](#-key-metrics)
- [Dashboards](#-dashboards)
- [Key Insights](#-key-insights)
- [Data Source](#-data-source)
- [Repository Structure](#-repository-structure)
- [How to Use](#-how-to-use)
- [Recommendations](#-recommendations)

---

## 🎯 Project Overview

Customer churn is a critical challenge for telecommunications companies. This project analyzes customer behavior, service subscriptions, and demographics to identify churn patterns and at-risk customers.

### Objectives

1. **Quantify Churn Impact** - Measure lost revenue and at-risk customers
2. **Identify Risk Factors** - Determine which factors correlate with churn
3. **Segment Analysis** - Analyze churn by contract type, payment method, and services
4. **Enable Action** - Provide insights for targeted retention strategies

---

## 📈 Key Metrics

### Overall Performance

| Metric | Value |
|--------|-------|
| Total Customers | 7,043 |
| Churn Rate | 26.54% |
| Customers at Risk | 1,869 |
| Lost Revenue | $2.86M |
| Yearly Charges | $16.06M |
| Monthly Charges | $139.13K |

### Support Tickets

| Type | Count |
|------|-------|
| Tech Tickets | 2,955 |
| Admin Tickets | 3,632 |

---

## 📊 Dashboards

### Dashboard 1: Customer Churn Dashboard

![Customer Churn Dashboard]![Image](https://github.com/user-attachments/assets/ce8157f4-dad5-4319-9979-0f99b7135f46)

**Focus:** At-risk customer profile and service subscriptions

**Key Highlights:**
- 88.55% of at-risk customers are on month-to-month contracts
- 69.40% use Fiber optic internet
- 74.91% use paperless billing
- Average monthly charge: $74.44

---

### Dashboard 2: Customer Risk Analysis Dashboard

![Customer Risk Analysis Dashboard](telecom_customer_churn_analysis/Risk.jpg)

**Focus:** Churn rate analysis across different dimensions

**Interactive Filters:**
- Churn status (Yes/No/All)
- Internet Services
- Age range slider (0-72)
- Contract type

---

## 💡 Key Insights

### 🔴 High-Risk Factors

1. **Month-to-Month Contracts** - 42.71% churn rate vs 2.83% for two-year contracts
2. **Fiber Optic Users** - 41.89% churn rate (highest among internet services)
3. **Electronic Check Payments** - 45.29% churn rate (highest payment method)
4. **New Customers (<1 Year)** - 47.44% churn rate in first year

### 🟢 Low-Risk Factors

1. **Long-term Contracts** - Two-year contracts: only 2.83% churn
2. **Tenure** - 6+ years: only 6.61% churn rate
3. **DSL Users** - 18.96% churn rate (lower than Fiber)

---

## 📁 Data Source

### Dataset: Customer Churn Dataset

| Field | Description |
|-------|-------------|
| CustomerID | Unique customer identifier |
| Gender | Male/Female |
| SeniorCitizen | Yes/No |
| Partner | Has partner (Yes/No) |
| Dependents | Has dependents (Yes/No) |
| Tenure | Months with company |
| Contract | Month-to-month/One year/Two year |
| MonthlyCharges | Monthly charge amount |
| TotalCharges | Total charges to date |
| Churn | Yes/No |

---

## 📂 Repository Structure

```
telecom_customer_churn_analysis/
│
├── README.md                          # Project documentation
├── LICENSE                            # MIT License
│
├── dashboards/
│   └── Customer_churn_dashboard.pbix  # Power BI file
│
├── data/
│   └── Customer_Churn_Dataset.xlsx    # Source data
│
├── screenshots/
│   ├── Customer_churn_dashboard_screenshot.jpg
│   └── Customer_risk_analysis_dashboard_screenshot_.jpg
│
└── docs/
    └── data_dictionary.md             # Field descriptions
```

---

## 🚀 How to Use

### Prerequisites

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free download)
- Microsoft Excel (for viewing source data)

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/csfrost/telecom_customer_churn_analysis.git
   ```

2. **Open the dashboard**
   - Launch Power BI Desktop
   - Open `dashboards/Customer_churn_dashboard.pbix`

3. **Explore the data**
   - Use slicers to filter by Churn status, Internet Service, Contract type
   - Hover over visuals for detailed tooltips
   - Click on chart elements to cross-filter

---

## ✅ Recommendations

Based on the analysis, here are strategic recommendations:

| Priority | Action | Target Segment | Expected Impact |
|----------|--------|----------------|-----------------|
| 🔴 High | Offer contract upgrade incentives | Month-to-month customers | Reduce 42.71% churn |
| 🔴 High | Improve Fiber optic service quality | Fiber customers | Address 41.89% churn |
| 🟡 Medium | Promote auto-pay/credit card | Electronic check users | Reduce 45.29% churn |
| 🟡 Medium | Enhanced onboarding program | New customers (<1 year) | Reduce 47.44% churn |

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  <i>"Retention is the new acquisition. Understanding why customers leave is the first step to making them stay."</i>
</p>

<p align="center">
  Made with 📊 by csfrost
</p>
