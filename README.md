# Customer Churn Prediction

Machine learning project for predicting customer churn in a telecom company.

## Business objective

The goal is to identify customers who are likely to leave the company so that a retention team can contact them before churn occurs.

## Dataset

The project uses the [Iranian Churn dataset](https://archive.ics.uci.edu/dataset/563/iranian%2Bchurn%2Bdataset) from the UCI Machine Learning Repository.

- 3,150 customers
- 13 input features
- Binary target: **Churn**
- Customer activity collected over 12 months
- No missing values reported by the dataset provider

Download the official dataset and save it locally as:

    data/customer_churn.csv

The raw CSV file is excluded from Git.

## Planned workflow

1. Data inspection and quality checks
2. Exploratory data analysis
3. Train and test split
4. Baseline model
5. Logistic Regression
6. Tree-based models
7. Model comparison
8. Classification threshold selection
9. Feature importance and business interpretation
10. Final recommendations

## Project structure

    customer-churn-prediction/
    ├── data/
    │   ├── README.md
    │   └── customer_churn.csv
    ├── images/
    ├── models/
    ├── notebooks/
    │   └── customer_churn_analysis.ipynb
    ├── .gitignore
    ├── README.md
    └── requirements.txt

## Installation

    pip install -r requirements.txt

## Status

Project in development.

## Dataset attribution

*Iranian Churn* (2020), UCI Machine Learning Repository.  
DOI: [10.24432/C5JW3Z](https://doi.org/10.24432/C5JW3Z). Licensed under CC BY 4.0.

