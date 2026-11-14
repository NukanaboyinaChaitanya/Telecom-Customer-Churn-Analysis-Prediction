# 📉 Telecom Customer Churn Analysis & Prediction

An end-to-end telecom customer churn analytics project using **SQL Server, Power BI, and Python**. This project analyzes customer behavior, identifies churn patterns, builds machine learning models, and provides business recommendations to improve customer retention.

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Business Problem](#-business-problem)
- [Tech Stack](#-tech-stack)
- [Repository Structure](#-repository-structure)
- [Dataset](#-dataset)
- [Project Workflow](#-project-workflow)
- [Power BI Dashboard](#-power-bi-dashboard)
- [Key Business Insights](#-key-business-insights)
- [Machine Learning Model](#-machine-learning-model)
- [Feature Importance](#-feature-importance)
- [Business Recommendations](#-business-recommendations)
- [How to Run](#-how-to-run)
- [Limitations & Future Improvements](#-limitations--future-improvements)

---

## 🔍 Project Overview

This project combines SQL-based data analysis, Power BI visualization, and Python-based machine learning to understand telecom customer churn.

| Metric | Value |
|---|---:|
| Total customers | 6,418 |
| Existing customers | 6,007 |
| New joiners | 411 |
| Churned customers | 1,732 |
| Churn rate among existing customers | 28.83% |
| Total dataset columns | 32 |
| Best-performing model by ROC-AUC | Logistic Regression |
| ROC-AUC | 0.872 |

### Project Objectives

1. Identify customer segments with high churn rates.
2. Understand the major reasons behind customer churn.
3. Analyze customer behavior and service usage.
4. Build a machine learning model to predict potential churners.
5. Provide actionable business recommendations for customer retention.

---

## 🎯 Business Problem

Customer retention is important for telecom companies because acquiring new customers can be more expensive than retaining existing ones.

The objective of this project is to identify customers who are likely to leave the service, understand the factors associated with churn, and help businesses develop targeted retention strategies.

---

## 🛠 Tech Stack

| Category | Technologies |
|---|---|
| Programming | Python |
| Database | SQL Server |
| Data Analysis | Pandas, NumPy |
| Data Visualization | Matplotlib, Seaborn, Power BI |
| Machine Learning | Scikit-learn |
| Algorithms | Logistic Regression, Random Forest |
| Environment | Jupyter Notebook |

---

## 📁 Repository Structure

```text
Telecom-Customer-Churn-Analysis-Prediction/
│
├── Data/
│   └── Customer_Data.csv
│
├── SQL/
│   └── churn_queries.sql
│
├── Notebook/
│   └── customerchurnprediction.ipynb
│
├── PowerBI/
│   └── Churn_Dashboard.pbix
│
├── Images/
│   └── dashboard.png
│
└── README.md
```

*Update folder and file names to match your actual GitHub repository.*

---

## 📊 Dataset

The dataset contains 6,418 customer records and 32 columns.

### Main Feature Categories

| Category | Features |
|---|---|
| Demographics | Customer_ID, Gender, Age, Married, State |
| Customer Engagement | Number_of_Referrals, Tenure_in_Months, Value_Deal |
| Phone Services | Phone_Service, Multiple_Lines |
| Internet Services | Internet_Service, Internet_Type, Online_Security, Online_Backup, Device_Protection_Plan, Premium_Support, Streaming_TV, Streaming_Movies, Streaming_Music, Unlimited_Data |
| Billing | Contract, Paperless_Billing, Payment_Method, Monthly_Charge, Total_Charges, Total_Revenue |
| Customer Outcome | Customer_Status, Churn_Category, Churn_Reason |

### Customer Status Distribution

| Customer Status | Count |
|---|---:|
| Stayed | 4,275 |
| Churned | 1,732 |
| Joined | 411 |
| **Total** | **6,418** |

For machine learning, only existing customers with a known outcome (`Stayed` or `Churned`) were considered. New joiners were excluded because their future churn status is not yet known.

---

## ⚙️ Project Workflow

### 1. SQL Server – Data Profiling and Analysis

- Analyzed customer distributions across gender, contract, state, and customer status.
- Performed missing-value checks across all 32 columns.
- Cleaned missing values in customer service-related fields.
- Created a production table named `prod_churn`.
- Created analytical views:
  - `vw_ChurnData` – customers who stayed or churned.
  - `vw_JoinData` – newly joined customers.

### 2. Python – Data Cleaning

Performed data cleaning using Pandas.

| Data Issue | Finding | Action Taken |
|---|---|---|
| Internet service-related missing values | 1,390 rows | Filled with `No Internet Service` |
| Multiple_Lines missing values | 622 rows | Filled with `No Phone Service` |
| Value_Deal missing values | 3,548 rows | Filled with `No Deal` |
| Negative Monthly_Charge | 107 invalid values | Replaced with median of valid charges |
| Duplicate records | 0 | No action required |

Missing values in `Churn_Category` and `Churn_Reason` were retained because these fields are applicable only to churned customers.

### 3. Exploratory Data Analysis (EDA)

Analyzed churn rates across:

- Contract types
- Tenure groups
- Payment methods
- Internet types
- Gender and age groups
- Marital status
- Additional services

Created visualizations including customer distribution charts, churn-rate comparisons, and a numerical correlation heatmap.

### 4. Machine Learning Preparation

- Selected 6,007 existing customers.
- Removed `Customer_ID` and post-outcome fields `Churn_Category` and `Churn_Reason`.
- Excluded cumulative billing columns from model predictors.
- Split data into training and testing sets using stratified sampling (80:20).
- Applied One-Hot Encoding to categorical features.
- Applied StandardScaler to numerical features.
- Fitted preprocessing transformations on training data only.

### 5. Model Building

Trained and compared three classification models:

- Logistic Regression
- Balanced Logistic Regression
- Random Forest

Evaluated the models using accuracy, precision, recall, F1-score, confusion matrices, and ROC-AUC.

---

## 📈 Power BI Dashboard

![Telecom Customer Churn Dashboard](Images/dashboard.png)

### KPI Cards

- Total Customers
- New Joiners
- Total Churn
- Churn Rate

### Dashboard Visualizations

- Total Churn by Gender
- Total Customers and Churn Rate by Age Group
- Total Churn by Churn Category
- Churn Rate by State
- Churn Rate by Contract
- Churn Rate by Payment Method
- Total Customers and Churn Rate by Tenure Group
- Churn Rate by Internet Type
- Churn by Additional Services

### Interactive Filters

- Monthly Charge Range
- Married Status

---

## 💡 Key Business Insights

### 1. Contract Type

Contract type is one of the strongest churn indicators.

- Month-to-Month: 52.38% churn rate.
- One-Year: 11.23% churn rate.
- Two-Year: 2.77% churn rate.

### 2. Payment Method

- Mailed Check: 42.86% churn rate.
- Bank Withdrawal: 36.05% churn rate.
- Credit Card: 16.16% churn rate.

### 3. Internet Type

- Fiber Optic: 42.47% churn rate.
- Cable: 27.57% churn rate.
- DSL: 20.82% churn rate.
- No Internet Service: 8.91% churn rate.

### 4. Additional Services

Customers without certain additional services showed higher churn rates.

- Without Online Security: 45.19%.
- With Online Security: 14.94%.
- Without Premium Support: 45.00%.
- With Premium Support: 15.77%.

### 5. Age Group

Senior customers showed a higher churn rate than younger and middle-aged customers.

### 6. Geography

Jammu & Kashmir showed the highest churn rate at approximately 57.2%, followed by Assam and Jharkhand.

### 7. Competitor-Related Churn

Competitors were the leading reported churn category:

- Competitor: 761 customers.
- Attitude: 301 customers.
- Dissatisfaction: 300 customers.
- Price: 196 customers.

---

## 🤖 Machine Learning Model

### Model Comparison

The following results were obtained on the test dataset using the default classification threshold of 0.50.

| Model | Accuracy | Churn Precision | Churn Recall | Churn F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 82% | 70% | 67% | 68% | **0.872** |
| Random Forest | 82% | 73% | 60% | 66% | 0.868 |
| Balanced Logistic Regression | 78% | 58% | 82% | 68% | 0.871 |

### Model Selection

Logistic Regression was selected based on its highest ROC-AUC score of approximately 0.872 and its interpretability.

### Classification Threshold Tuning

The default threshold of 0.50 was compared with lower thresholds to improve churn detection.

| Threshold | Churn Precision | Churn Recall | Churn F1-score |
|---|---:|---:|---:|
| 0.50 | 70% | 67% | 68% |
| **0.40** | **65%** | **76%** | **70%** |
| 0.35 | 62% | 78% | 69% |

### Results at Threshold 0.40

| Metric | Result |
|---|---:|
| Accuracy | 81% |
| Churn Precision | 65% |
| Churn Recall | 76% |
| Churn F1-score | 70% |

The 0.40 threshold was selected as an exploratory candidate because it achieved the highest churn F1-score among the three tested thresholds.

**Note:** Threshold selection and evaluation were performed using the same test set. Therefore, these results should be treated as exploratory rather than an unbiased final performance estimate. A separate validation process is needed before deployment.

---

## 📌 Feature Importance Analysis

Logistic Regression coefficients were analyzed to understand which features were associated with higher or lower predicted churn.

### Features Associated with Higher Churn

- Month-to-Month Contract
- Higher Monthly Charges
- Value Deal 5
- Certain geographic regions, including Jammu & Kashmir, Assam, and Jharkhand
- Mailed Check Payment Method

### Features Associated with Lower Churn

- Two-Year Contract
- Online Security
- Premium Support
- Credit Card Payment Method
- Value Deals 2 and 3

These coefficients represent model associations relative to the relevant reference categories. They do not establish causation.

---

## 🚀 Business Recommendations

1. **Encourage Long-Term Contracts:** Offer incentives to month-to-month customers to upgrade to one-year or two-year plans.
2. **Improve Customer Support:** Promote Online Security and Premium Support services.
3. **Review Pricing Strategies:** Investigate customers with higher monthly charges.
4. **Address Competitor-Related Churn:** Develop competitive pricing and customer retention campaigns.
5. **Improve Fiber Optic Services:** Investigate service quality and pricing for Fiber Optic customers.
6. **Regional Retention Strategies:** Investigate customer experiences in high-churn states.
7. **Target At-Risk Customers:** Use churn predictions to prioritize customer retention campaigns.

---

## ▶️ How to Run

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/Telecom-Customer-Churn-Analysis-Prediction.git

cd Telecom-Customer-Churn-Analysis-Prediction
```

### 2. Install Required Python Libraries

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### 3. Run the Python Notebook

```bash
jupyter notebook Notebook/customerchurnprediction.ipynb
```

Update the dataset path in the notebook to match the repository structure.

### 4. Run SQL Queries

- Import the CSV dataset into SQL Server.
- Execute the SQL scripts in the `SQL` folder.
- Create the required analytical tables and views.

### 5. Open the Power BI Dashboard

- Open `Churn_Dashboard.pbix` using Power BI Desktop.
- Configure the SQL Server data source.
- Refresh the dashboard.

---

## ⚠️ Limitations & Future Improvements

### Current Limitations

- The Power BI churn rate is 27.0%, while the Python churn rate is 28.83%. The difference is due to different denominators: the dashboard includes all 6,418 customers, while Python considers only 6,007 existing customers.
- SQL and Python cleaning use slightly different replacement labels.
- Age and tenure group definitions differ between the dashboard and Python analysis.
- Threshold selection was performed on the test set, so independent validation is required.

### Future Improvements

- Use a separate validation set or cross-validation for threshold selection.
- Build a complete preprocessing and modeling pipeline using `ColumnTransformer` and `Pipeline`.
- Experiment with XGBoost and LightGBM.
- Perform hyperparameter tuning.
- Use SHAP for model explainability.
- Develop a Streamlit application for interactive churn prediction.
- Deploy the model as an API for customer risk scoring.


---

⭐ If you find this project useful, consider giving the repository a star!
