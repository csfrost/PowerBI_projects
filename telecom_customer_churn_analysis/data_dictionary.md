# 📖 Data Dictionary

## Telecom Customer Churn Dataset

This document describes all fields in the customer churn dataset used for the Power BI analysis.

---

## 📊 Dataset Overview

| Property | Value |
|----------|-------|
| Total Records | 7,043 customers |
| Total Fields | 21 columns |
| File Format | Excel (.xlsx) |
| Churn Rate | 26.54% |

---

## 🔤 Field Descriptions

### Customer Identification

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `CustomerID` | String | Unique identifier for each customer | "7590-VHVEG" |

---

### Demographics

| Field | Type | Values | Description |
|-------|------|--------|-------------|
| `Gender` | Categorical | Male, Female | Customer's gender |
| `SeniorCitizen` | Binary | 0, 1 | Whether customer is 65+ years old |
| `Partner` | Categorical | Yes, No | Whether customer has a partner |
| `Dependents` | Categorical | Yes, No | Whether customer has dependents |

**Dashboard Stats (At-Risk Customers):**
- Gender: 50.24% Male, 49.76% Female
- Senior Citizen: 25%
- Partners: 36%
- Dependents: 17%

---

### Account Information

| Field | Type | Description | Range |
|-------|------|-------------|-------|
| `Tenure` | Integer | Number of months as customer | 0-72 months |
| `Contract` | Categorical | Type of contract | Month-to-month, One year, Two year |
| `PaperlessBilling` | Categorical | Electronic billing enabled | Yes, No |
| `PaymentMethod` | Categorical | How customer pays | See below |

**Payment Methods:**
- Electronic check (57.30% of at-risk)
- Mailed check (16.48%)
- Bank transfer (automatic) (13.80%)
- Credit card (automatic) (12.41%)

**Contract Distribution (At-Risk):**
- Month-to-month: 88.55%
- One year: 8.88%
- Two year: 2.57%

---

### Services Subscribed

| Field | Type | Values | Description |
|-------|------|--------|-------------|
| `PhoneService` | Categorical | Yes, No | Has phone service |
| `MultipleLines` | Categorical | Yes, No, No phone service | Has multiple phone lines |
| `InternetService` | Categorical | DSL, Fiber optic, No | Type of internet service |
| `OnlineSecurity` | Categorical | Yes, No, No internet service | Has online security add-on |
| `OnlineBackup` | Categorical | Yes, No, No internet service | Has online backup add-on |
| `DeviceProtection` | Categorical | Yes, No, No internet service | Has device protection add-on |
| `TechSupport` | Categorical | Yes, No, No internet service | Has tech support add-on |
| `StreamingTV` | Categorical | Yes, No, No internet service | Has streaming TV add-on |
| `StreamingMovies` | Categorical | Yes, No, No internet service | Has streaming movies add-on |

**Service Subscription Rates (At-Risk Customers):**

| Service | Subscription % |
|---------|---------------|
| Phone Service | 91% |
| Streaming TV | 44% |
| Streaming Movies | 44% |
| Device Protection | 29% |
| Online Backup | 28% |
| Tech Support | 17% |
| Online Security | 16% |

---

### Financial Information

| Field | Type | Description | Dashboard Stats |
|-------|------|-------------|-----------------|
| `MonthlyCharges` | Decimal | Current monthly charge | Avg: $74.44 |
| `TotalCharges` | Decimal | Total amount charged | Avg: $1,531.80 |

**Revenue Impact:**
- Lost Revenue (Churned): $2.86M
- Monthly Charges (Total): $139.13K
- Yearly Charges (Total): $16.06M

---

### Target Variable

| Field | Type | Values | Description |
|-------|------|--------|-------------|
| `Churn` | Categorical | Yes, No | Whether customer left the company |

**Churn Statistics:**
- Total Customers: 7,043
- Churned: 1,869 (26.54%)
- Retained: 5,174 (73.46%)

---

## 📈 Calculated Fields (DAX)

These measures were created in Power BI for analysis:

### Churn Rate
```dax
Churn Rate = 
DIVIDE(
    COUNTROWS(FILTER(Customers, Customers[Churn] = "Yes")),
    COUNTROWS(Customers),
    0
)
```

### Lost Revenue
```dax
Lost Revenue = 
CALCULATE(
    SUM(Customers[TotalCharges]),
    Customers[Churn] = "Yes"
)
```

### Customers at Risk
```dax
Customers at Risk = 
COUNTROWS(FILTER(Customers, Customers[Churn] = "Yes"))
```

### Average Monthly Charges
```dax
Avg Monthly Charges = 
AVERAGE(Customers[MonthlyCharges])
```

---

## 🔗 Relationships

The dataset is a single flat table (no relationships required). All analysis is performed on the `Customer_Churn_Dataset` table.

---

## 📊 Churn Analysis by Dimension

### By Internet Service

| Service | Customers | Churn Rate | Revenue at Risk |
|---------|-----------|------------|-----------------|
| Fiber optic | 3,096 (43.96%) | 41.89% | $283.28K/month |
| DSL | 2,421 (34.37%) | 18.96% | $140.67K/month |
| No Internet | 1,526 (21.67%) | 7.40% | $32.17K/month |

### By Contract Type

| Contract | Customers | Churn Rate |
|----------|-----------|------------|
| Month-to-month | ~3,875 | 42.71% |
| One year | ~1,473 | 11.27% |
| Two year | ~1,695 | 2.83% |

### By Tenure (Subscription Time)

| Tenure | Churn Rate | Revenue Impact |
|--------|------------|----------------|
| <1 Year | 47.44% | $0.12M |
| 1-2 Years | 28.71% | $0.05M |
| 2-3 Years | 21.63% | $0.06M |
| 3-4 Years | 14.42% | $0.05M |
| 4-5 Years | 6.61% | $0.11M |
| 5-6 Years | ~6% | ~$0.11M |

### By Payment Method

| Method | Churn Rate | Lost Revenue |
|--------|------------|--------------|
| Electronic check | 45.29% | $0.18M |
| Mailed check | 19.11% | $0.07M |
| Bank transfer | 16.71% | $0.10M |
| Credit card | 15.24% | $0.10M |

---

## 📝 Data Quality Notes

1. **No Missing Values** - Dataset is complete
2. **Consistent Formatting** - All categorical values standardized
3. **Logical Constraints** - Services dependent on internet show "No internet service" appropriately
4. **Tenure Range** - 0-72 months (6 years maximum)

---

## 🎯 Key Segments for Analysis

### High-Risk Profile
- Month-to-month contract
- Fiber optic internet
- Electronic check payment
- Tenure < 1 year
- No tech support or online security

### Low-Risk Profile
- Two-year contract
- DSL or no internet
- Credit card auto-pay
- Tenure > 3 years
- Multiple add-on services
