# Telecom Customer Churn Analysis & Prediction

## Project Overview
Customer churn is a major challenge for telecom companies because losing customers directly impacts revenue.  
This project analyzes telecom customer data to identify churn patterns and predict customers likely to churn using machine learning.

The project integrates SQL for data preparation, Power BI for visualization, and machine learning for predictive analysis.

---

## Project Workflow

Customer Dataset  
↓  
SQL Data Cleaning (SQL Server)  
↓  
Power BI Dashboard (Churn Analysis)  
↓  
Feature Engineering  
↓  
Machine Learning Model (Random Forest)  
↓  
Prediction of High-Risk Customers  

---

## Tools and Technologies

- SQL Server (SSMS)
- Power BI
- Python
- Pandas
- NumPy
- Scikit-learn
- Random Forest Classifier

---

## Dataset

The dataset contains telecom customer information including:

- Customer ID
- Gender
- Age
- Marital Status
- State
- Tenure in Months
- Contract Type
- Internet Service
- Payment Method
- Monthly Charges
- Customer Status
- Churn Category
- Churn Reason

Total Customers: 6418

---

## Power BI Dashboard

The Power BI dashboard provides insights into customer churn patterns.

Key KPIs

- Total Customers: 6418
- Total Churn Customers: 1732
- Churn Rate: 27%
- New Joiners: 411

Dashboard analysis includes

- Churn by Age Group
- Churn by Tenure Group
- Churn by Contract Type
- Churn by Payment Method
- Churn by Internet Service
- Churn by Customer Services

---

## Machine Learning Model

A Random Forest classification model was used to identify customers likely to churn.

Steps performed

- Data preprocessing
- Feature selection
- Train-test split
- Model training
- Prediction of churn-risk customers

The model predicts customers who show behavior patterns similar to previously churned customers.

---

## Key Insights

- Customers with shorter tenure show higher churn likelihood.
- Month-to-month contracts have higher churn rates compared to long-term contracts.
- Customers with higher monthly charges tend to churn more frequently.
- Certain internet service types show higher churn patterns.

---

## Business Impact

This project helps telecom companies

- Identify customers at risk of leaving
- Understand factors influencing churn
- Implement targeted retention strategies
- Improve customer satisfaction and reduce revenue loss

---

## Project Structure

telecom-churn-analysis

data  
Customer_Data.csv  
churndata.csv  
joindata.csv  
prediction.csv  

sql  
SQLQuery1.sql  

notebooks  
Churn Prediction.ipynb  

dashboard  
Churn Analysis.pbix  
Churn Analysis Dashboard.pdf  

docs  
Telecom Customer Churn Analysis.pdf  

README.md

---

## How to Run the Project

1. Load the dataset from the data folder.
2. Execute SQL queries from the sql folder to prepare cleaned datasets.
3. Open the Power BI dashboard (.pbix) to explore churn insights.
4. Run the Churn Prediction.ipynb notebook to train the machine learning model.
5. The model outputs predicted churn customers in prediction.csv.

---

## Author

Chaitanya 
Aspiring Data Analyst | AIML Student
