# Customer Churn Prediction

A data science project that explores customer churn in a telecom company and builds a model to help a retention team prioritize outreach.

## Business objective

The goal is to identify customers at risk of leaving before the end of the observation period. A useful model should find customers who will churn while keeping unnecessary outreach manageable. In this project, `Churn = 1` means the customer left and `Churn = 0` means the customer stayed.

## Dataset

The project uses the [Iranian Churn dataset](https://archive.ics.uci.edu/dataset/563/iranian%2Bchurn%2Bdataset) from the UCI Machine Learning Repository. It contains 3,150 customer records, 13 input features, and the `Churn` target. The features summarize the first nine months of customer data; churn is recorded at the end of month 12. This leaves a three-month planning gap between the observed features and the outcome.

The data includes complaints, call and SMS activity, subscription length, tariff information, customer value, and other customer characteristics. No missing values were found. The CSV is downloaded separately and is excluded from Git.

## Method

1. Inspect the dataset and remove 300 exact duplicate rows, leaving 2,850 records.
2. Explore how complaints and service usage differ between customers who stayed and those who left.
3. Exclude `Status` from the initial models and split the cleaned data into 80% training and 20% test data, preserving the churn rate in each part.
4. Compare a majority-class baseline, logistic regression, and a random forest. Compare the two trained models using five-fold cross-validation on the training data.
5. Explore decision thresholds of `0.5` and `0.7` for logistic regression on a separate validation portion of the training data.
6. Evaluate the selected random forest on the test data using its default decision threshold and save the fitted model locally.

## Exploratory findings

- The churn rate after removing exact duplicates is **15.6%**.
- **82.6%** of customers with complaints churned, compared with **9.8%** of customers without complaints.
- Customers who churned made fewer calls (median **29** versus **64**), spent less time on calls (median **1,256.5** versus **3,596.5** seconds), and sent fewer SMS messages (median **11** versus **26**).
- Median `Customer Value` was **102.565** for customers who churned and **271.540** for customers who stayed.

These are associations in this dataset; they do not establish what caused a customer to leave. The calculation behind `Customer Value` is not specified in enough detail to interpret it as revenue.

## Model results

The table below reports results on the same 570-row test set. Precision and recall refer to `Churn = 1`.

| Model | Accuracy | Precision | Recall |
| --- | ---: | ---: | ---: |
| Majority-class baseline | 0.844 | 0.000 | 0.000 |
| Logistic regression | 0.807 | 0.441 | 0.876 |
| Random forest | **0.863** | **0.536** | **0.910** |

The random forest identified **81 of 89** customers who churned. It missed **8** and flagged **70** customers who stayed. Among its 151 alerts, 81 were correct. By comparison, the majority-class baseline predicted that everyone would stay and found no churned customers.

On the training data, five-fold cross-validation gave the random forest mean precision **0.545** and mean recall **0.891**; logistic regression achieved **0.464** and **0.865**, respectively.

For logistic regression, a threshold of `0.5` on a validation subset found 84 of 89 churned customers but produced 100 false alerts. At `0.7`, it found 66 and produced 44 false alerts. This illustrates the trade-off between finding more customers at risk and making fewer unnecessary contacts. The final random forest results above use its default decision threshold; the logistic regression threshold comparison was not applied to that model.

## How to run

1. Clone or download this repository.
2. Download the CSV from the [UCI dataset page](https://archive.ics.uci.edu/dataset/563/iranian%2Bchurn%2Bdataset), extract it, and save it as `data/customer_churn.csv`.
3. From the project root, install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Open `notebooks/customer_churn_analysis.ipynb` in VS Code or Jupyter and run the cells from top to bottom.

For Google Colab, place the project folder at `MyDrive/projects/customer-churn-prediction` and make sure the CSV is at `MyDrive/projects/customer-churn-prediction/data/customer_churn.csv`. The notebook mounts Google Drive when it runs in Colab. The trained model is saved to `models/churn_forest.joblib` and is excluded from Git; running the notebook recreates it.

## Project structure

```text
customer-churn-prediction/
├── data/
│   ├── README.md
│   └── customer_churn.csv       # Download separately; ignored by Git
├── images/
├── models/                      # Generated model files; ignored by Git
├── notebooks/
│   └── customer_churn_analysis.ipynb
├── .gitignore
├── README.md
└── requirements.txt
```

## Limitations and next steps

- The dataset has no customer ID, so identical rows cannot be confirmed to represent the same person. The project removes exact duplicates and documents that choice.
- `Status` is excluded from the initial models because its meaning and usefulness at the intended decision point need additional scrutiny. Its exclusion does not prove that it leaks the target.
- The results come from one historical dataset. Performance for a different telecom operator or a later period has not been established.
- The best outreach threshold depends on the cost of contacting customers and the cost of losing them; those costs are unavailable here. Model scores should not be treated as calibrated churn probabilities without checking calibration.

## Dataset attribution

*Iranian Churn* (2020), UCI Machine Learning Repository. [DOI: 10.24432/C5JW3Z](https://doi.org/10.24432/C5JW3Z). Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).