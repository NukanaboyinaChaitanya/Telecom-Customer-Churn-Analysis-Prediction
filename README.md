# 📉 Telecom Customer Churn Analysis & Prediction

An end-to-end churn analytics project on an Indian telecom customer dataset, combining **SQL Server** (data profiling & cleaning), **Power BI** (interactive dashboard) and **Python** (EDA + machine learning) to understand *why* customers leave and *who* is likely to leave next.

---

## 📌 Table of Contents
- [Project Overview](#-project-overview)
- [Business Problem](#-business-problem)
- [Tech Stack](#-tech-stack)
- [Repository Structure](#-repository-structure)
- [Dataset](#-dataset)
- [Workflow](#-workflow)
- [Power BI Dashboard](#-power-bi-dashboard)
- [Key Insights](#-key-insights)
- [Machine Learning Model](#-machine-learning-model)
- [Business Recommendations](#-business-recommendations)
- [How to Run](#-how-to-run)
- [Limitations & Future Work](#-limitations--future-work)

---

## 🔍 Project Overview

| Metric | Value |
|---|---|
| Total customers | **6,418** |
| New joiners | **411** |
| Churned customers | **1,732** |
| Churn rate (dashboard) | **27.0%** |
| Features | 32 raw columns |
| Best model | Logistic Regression (ROC-AUC **0.872**) |

The project answers three questions:
1. **Who** is churning? (demographics, contract, services)
2. **Why** are they churning? (churn category / reason)
3. **Who will churn next?** (predictive model with a tuned decision threshold)

---

## 🎯 Business Problem

Customer acquisition costs far more than retention. The goal is to identify the main drivers of churn so the business can run targeted retention campaigns and focus offers on high-risk segments instead of discounting across the board.

---

## 🛠 Tech Stack

| Layer | Tools |
|---|---|
| Database / ETL | SQL Server (T-SQL) |
| Visualization | Power BI |
| Analysis & ML | Python, Pandas, NumPy, Matplotlib, Seaborn, scikit-learn |
| Environment | Jupyter Notebook |

---

## 📁 Repository Structure

```
Telecom-Customer-Churn-Analysis-Prediction/
│
├── Data/
│   └── Customer_Data.csv              # Raw dataset (6,418 rows × 32 columns)
│
├── SQL/
│   └── churn_queries.sql              # Profiling, null checks, prod table & Power BI views
│
├── Notebook/
│   └── customerchurnprediction.ipynb  # Cleaning, EDA, modelling, evaluation
│
├── PowerBI/
│   └── Churn_Dashboard.pbix           # Power BI report
│
├── Images/
│   └── dashboard.png                  # Dashboard screenshot
│
└── README.md
```

> Adjust folder and file names to match your actual repo layout.

---

## 📊 Dataset

Each row is one customer. Columns fall into these groups:

| Group | Columns |
|---|---|
| **Demographics** | Customer_ID, Gender, Age, Married, State |
| **Engagement** | Number_of_Referrals, Tenure_in_Months, Value_Deal |
| **Phone services** | Phone_Service, Multiple_Lines |
| **Internet services** | Internet_Service, Internet_Type, Online_Security, Online_Backup, Device_Protection_Plan, Premium_Support, Streaming_TV, Streaming_Movies, Streaming_Music, Unlimited_Data |
| **Billing** | Contract, Paperless_Billing, Payment_Method, Monthly_Charge, Total_Charges, Total_Refunds, Total_Extra_Data_Charges, Total_Long_Distance_Charges, Total_Revenue |
| **Target / outcome** | Customer_Status (`Stayed`, `Churned`, `Joined`), Churn_Category, Churn_Reason |

**Customer status split:** Stayed 4,275 · Churned 1,732 · Joined 411

---

## ⚙️ Workflow

### 1. SQL – Profiling & Cleaning
- Profiled distributions of Gender, Contract, State and Customer_Status (with revenue share).
- Ran a null-count audit across all 32 columns.
- Replaced nulls with meaningful values and loaded the result into a production table `prod_churn`:
  - Service add-on fields → `'No'` / `'None'`
  - `Value_Deal` → `'None'`
  - `Churn_Category`, `Churn_Reason` → `'Others'`
- Created Power BI-ready views:
  - `vw_ChurnData` – customers with status `Churned` or `Stayed`
  - `vw_JoinData` – customers with status `Joined`

### 2. Python – Data Cleaning
| Issue | Finding | Fix |
|---|---|---|
| Internet add-ons null (1,390 rows) | Exactly the customers with `Internet_Service = No` | Filled with `No Internet Service` |
| `Multiple_Lines` null (622 rows) | Exactly the customers with `Phone_Service = No` | Filled with `No Phone Service` |
| `Value_Deal` null (3,548 rows) | Customer is not on any deal | Filled with `No Deal` |
| Negative `Monthly_Charge` | Values between −10 and −1 (invalid) | Replaced with median of valid charges |
| Duplicates | 0 | None needed |

`Churn_Category` / `Churn_Reason` nulls (4,686) are expected, since they only exist for churned customers.

### 3. Python – Exploratory Data Analysis
Churn rate computed across Contract, Tenure, Payment Method, Internet Type, Gender, Age Group, Marital Status and every add-on service, plus a correlation heatmap of numeric features.

### 4. Python – Modelling
- Restricted to `Stayed` vs `Churned` customers (6,007 rows). `Joined` customers have no outcome yet.
- 80/20 stratified train/test split (`random_state=42`).
- **Data leakage prevention:** dropped `Customer_ID`, `Churn_Category`, `Churn_Reason` and the cumulative billing columns (`Total_Charges`, `Total_Refunds`, `Total_Extra_Data_Charges`, `Total_Long_Distance_Charges`, `Total_Revenue`).
- One-hot encoding for 19 categorical features, standard scaling for 4 numeric features (fit on train only).
- Compared three models, then tuned the decision threshold.

---

## 📈 Power BI Dashboard

![Churn Dashboard](Images/dashboard.png)

**KPI cards:** Total Customers · New Joiners · Total Churn · Churn Rate
**Slicers:** Monthly Charge Range, Married

**Visuals:**
- Total Churn by Gender
- Total Customers & Churn Rate by Age Group
- Total Churn by Churn Category
- Churn Rate by State (Top 5)
- Churn Rate by Contract
- Churn Rate by Payment Method
- Total Customers & Churn Rate by Tenure Group
- Churn Rate by Internet Type
- Churn by Services (Yes vs No)

---

## 💡 Key Insights

**Contract type is the strongest signal**
- Month-to-month customers churn at roughly **46–52%**, versus about **11%** for one-year and under **3%** for two-year contracts.

**Competitors are the #1 reason for churn**
- **761** of the 1,732 churned customers (about 44%) left because of a competitor, followed by Attitude (301), Dissatisfaction (300) and Price (196).

**Payment method matters**
- Mailed check (~38–43%) and bank withdrawal (~34–36%) show far higher churn than credit card (~15–16%).

**Fiber Optic customers churn the most**
- Fiber Optic churn is around **41–42%**, versus DSL at ~19–21%. Customers with no internet service churn the least (~8–9%).

**Support & security services protect against churn**
- Customers *without* Online Security (45%) or Premium Support (45%) churn about **3× more** than those with them (15–16%).

**Seniors churn more**
- Customers over 50 have the highest churn (31% in the dashboard; 33% for the 50+ group in the notebook).

**Geography**
- Jammu & Kashmir shows an extreme churn rate of **57.2%**, followed by Assam (38.1%), Jharkhand (34.5%), Chhattisgarh (30.5%) and Delhi (29.9%).

**Weak or no signal**
- Gender, marital status and number of referrals show little difference in churn rate. Tenure group shows a fairly flat ~26–28% pattern.

---

## 🤖 Machine Learning Model

### Model comparison (test set, threshold 0.50)

| Model | Accuracy | Churn Precision | Churn Recall | Churn F1 | ROC-AUC |
|---|---|---|---|---|---|
| **Logistic Regression** | 0.82 | 0.70 | 0.67 | 0.68 | **0.872** |
| Random Forest (200 trees, balanced) | 0.82 | 0.73 | 0.60 | 0.66 | 0.868 |
| Logistic Regression (balanced) | 0.78 | 0.58 | 0.82 | 0.68 | 0.871 |

Logistic Regression was selected: it has the best ROC-AUC, it is simple, and its coefficients are directly interpretable.

### Threshold tuning

Since missing a churner costs more than contacting a loyal customer, the threshold was lowered to improve recall:

| Threshold | Precision | Recall | F1 |
|---|---|---|---|
| 0.50 | 0.70 | 0.67 | 0.68 |
| **0.40 (chosen)** | 0.65 | **0.76** | **0.70** |
| 0.35 | 0.62 | 0.78 | 0.69 |

**Final model at threshold 0.40:** Accuracy **81%**, Churn Recall **76%**, Churn Precision **65%**, F1 **0.70**.

### Top churn drivers (Logistic Regression coefficients)

| Increases churn risk ⬆️ | Decreases churn risk ⬇️ |
|---|---|
| Contract: Month-to-Month | Contract: Two Year |
| Higher Monthly Charge | Online Security: Yes |
| Value Deal 5 | Premium Support: Yes |
| State: Jammu & Kashmir, Assam, Jharkhand | Value Deals 2 and 3 |
| Payment: Mailed Check | Payment: Credit Card |

---

## 🚀 Business Recommendations

1. **Push contract upgrades.** Offer discounts or perks for moving month-to-month customers to 1 or 2-year plans.
2. **Bundle security and support.** Online Security and Premium Support are strongly linked with retention, so include them in plans or offer free trials.
3. **Counter competitors.** Competitor-driven churn is the largest category, so use competitive pricing, device offers and win-back campaigns.
4. **Encourage auto-pay by credit card.** Offer incentives to move customers off mailed check and bank withdrawal.
5. **Fix Fiber Optic experience.** Investigate service quality and pricing for this high-churn segment.
6. **Regional action plans.** Prioritize Jammu & Kashmir, Assam and Jharkhand for network and service reviews.
7. **Use the model for targeting.** Score active customers and contact those above the 0.40 churn probability threshold with retention offers.
8. **Review Deal 5.** It is associated with higher churn, so check its pricing and terms.

---

## ▶️ How to Run

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/Telecom-Customer-Churn-Analysis-Prediction.git
cd Telecom-Customer-Churn-Analysis-Prediction
```

### 2. Python notebook
```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
jupyter notebook Notebook/customerchurnprediction.ipynb
```
> ⚠️ Update the CSV path in the notebook's data-loading cell. It currently points to a local Windows path. Use a relative path such as `../Data/Customer_Data.csv`.

### 3. SQL Server
1. Import `Customer_Data.csv` into a table named `Customer_data`.
2. Run `SQL/churn_queries.sql` to profile the data, create `prod_churn` and build the views `vw_ChurnData` and `vw_JoinData`.

### 4. Power BI
1. Open the `.pbix` file in Power BI Desktop.
2. Point the data source to your SQL Server instance and refresh.

---

## ⚠️ Limitations & Future Work

- **Churn rate definition differs between tools.** The Power BI KPI (27.0%) divides churned customers by *all* customers (6,418, including new joiners). The notebook (28.83%) divides by *existing* customers only (Stayed + Churned = 6,007). Both are valid, but state which one you report.
- **Cleaning differs slightly between SQL and Python.** SQL fills nulls with `No` / `None`, while Python uses `No Internet Service` / `No Phone Service`. The SQL step also does not fix negative `Monthly_Charge` values, which the notebook handles.
- **Tenure and age bins differ** between the dashboard (5 tenure bins, 4 age bins) and the notebook (3 tenure bins, 3 age bins), so percentages are not directly comparable.
- Class imbalance (about 29% churn) is handled via threshold tuning rather than resampling.
- **Future work:**
  - Try gradient boosting (XGBoost / LightGBM) and cross-validation
  - Hyperparameter tuning and SHAP explanations
  - Add a customer lifetime value (CLV) layer to prioritize high-value at-risk customers
  - Save the trained model and scale it into a scoring pipeline or Streamlit app

---



---

⭐ If you found this project useful, consider giving it a star!
