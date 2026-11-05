# crisp_dm_telco_churn.ipynb

*A practical, end-to-end walkthrough of predicting customer churn with the CRISP-DM lifecycle*

## Introduction

Customer churn is rarely solved by training a classifier and reporting accuracy. A useful churn project must connect a business decision to data quality, modeling, evaluation, and deployment.

In this project, I used the Telco Customer Churn dataset to build a decision-support workflow for a telecom retention team. The objective was to estimate which customers are most likely to leave and prioritize them for human-reviewed retention support.

The project follows the six CRISP-DM phases: Business Understanding, Data Understanding, Data Preparation, Modeling, Evaluation, and Deployment.

## Business Understanding

The business problem is preventable customer loss. A retention team has limited time and cannot contact every customer, so the data-mining system should create a ranked list of customers who may need attention.

The technical objective is to estimate churn probability. The business success criteria are more specific than “high accuracy”: the model should identify a useful number of churners, maintain acceptable precision, and support a practical intervention threshold.

The model is a decision aid. A high-risk score should lead to human review, not automatic service denial or an assumption about why a customer is unhappy.

## Data Understanding

The dataset contains 7,043 customers and 21 columns. It includes customer demographics, tenure, contract type, internet and phone services, technical support, security services, monthly and total charges, and the target variable Churn.

The target is imbalanced: 1,869 customers churned and 5,174 did not. That is a 26.54% churn rate. Accuracy alone would be misleading because a model can appear accurate while failing to find enough churners.

One important data-quality issue is TotalCharges. It is stored as text and contains blank values. Those blanks must become missing numeric values before modeling.

## Data Preparation

The target was converted from Yes and No to 1 and 0. customerID was removed because it is an identifier rather than a behavioral feature. TotalCharges was converted to numeric, with invalid blanks handled as missing values.

The dataset was split into training and testing partitions using stratification. Numeric fields use median imputation and standardization. Categorical fields use most-frequent imputation and one-hot encoding. All transformations are fit only on training data, preventing test-set leakage.

## Modeling

Three models were compared: a majority-class baseline, balanced logistic regression, and balanced random forest. The baseline provides a minimum reference. Logistic regression was selected as the primary deployment candidate because its probability score is relatively transparent and easy to audit. Random forest was used as a nonlinear benchmark.

The logistic model achieved ROC-AUC of approximately 0.8413. At the default 0.50 threshold, churn recall was approximately 78.3% and churn precision was approximately 50.4%.

## Evaluation and Threshold Selection

ROC-AUC measures ranking performance across thresholds. Precision measures how many flagged customers actually churned. Recall measures how many actual churners were found. F1 summarizes the balance between precision and recall.

For the retention use case, the default threshold should not automatically be treated as final. I selected a demonstration threshold using an explicit rule: precision should be at least 0.65 while recall should be at least 0.35 whenever possible.

The selected threshold was approximately 0.7278. At that threshold, the model flagged 327 customers, with approximately 65.1% precision and 57.0% recall. A production team should replace this demonstration rule with a cost-based analysis using offer cost, customer value, intervention capacity, and the financial impact of false negatives.

## Deployment

The notebook saves the complete preprocessing and modeling pipeline as a joblib artifact. A Streamlit interface can accept a customer profile and display a churn probability along with a human-review recommendation.

Production controls should include model-version logging, input validation, probability calibration, drift monitoring, fairness checks, retention-offer acceptance tracking, periodic retraining, and human approval before action.

## Lessons Learned

CRISP-DM is a loop rather than a linear checklist. The business question shaped the metrics. The data-quality review changed the preparation pipeline. Evaluation shaped the operating threshold. Deployment introduced new monitoring requirements.

A technically accurate model is not automatically a useful business system. It becomes useful when its output is connected to a clearly defined action and its limitations are visible.

## Reproduce the Project

- GitHub: Replace with your public repository URL
- Google Colab: Replace with your Colab URL
- Walkthrough video: Replace with your YouTube URL

## References

- Dataset: https://www.kaggle.com/blastchar/telco-customer-churn
- Methodology: https://www.ibm.com/docs/en/spss-modeler/saas?topic=dm-crisp-help-overview
