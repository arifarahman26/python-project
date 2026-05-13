# Telco Customer Churn Prediction Project Report
## Project Overview
This project aimed to build a predictive model to identify customers at risk of churning from a telecommunications company. The primary goal was to leverage customer data to forecast churn behavior and provide actionable insights for retention strategies.

## Areas We Have Worked On
**Comprehensive Data Analysis:** Analysis of customer records with 25 features to understand patterns.
**Multiple ML Models:** Comparison of Logistic Regression, Random Forest, Decision Tree, and SVC for churn prediction.
**Feature Importance Analysis:** Identification of key drivers of customer churn to guide business strategies.
**Production-Ready Prediction Function:** Developed a function for real-time churn predictions.
**Business Insights:** Provided actionable recommendations to reduce churn based on model findings.

## Dataset
**Source**: Kaggle - Telco Customer Churn (https://www.kaggle.com/datasets/blastchar/telco-customer-churn)
**Dataset File:** telco_churn.csv
**Records:** 7,395 customers (before duplicate removal, 7,253 unique records after cleaning).
**Features:** 21 (including 20 predictors and 1 target variable Churn).
**Target Variable:** Churn (0 = Not Churned, 1 = Churned).

## Features Include:
**Customer Demographics**: gender, SeniorCitizen.
**Account Information:** Partner, Dependents, Contract, PaperlessBilling, PaymentMethod.
**Service Information:** PhoneService, MultipleLines, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies.
**Usage and Billing:** tenure, MonthlyCharges, TotalCharges.

## Data Preprocessing & Cleaning
**Data Loading:** The telco_churn.csv dataset was loaded, containing 7395 records and 21 features.
**Missing Values:** Missing values were identified in TotalCharges (1.23%), MonthlyCharges (1.14%), tenure (0.99%), and Contract (0.96%). Numerical features were imputed using the median, and categorical features using the mode.
**Duplicates:** 131 duplicate rows were identified and removed, reducing the dataset to 7253 unique records.
**Outlier Treatment:** Outliers in numerical features (MonthlyCharges, tenure, TotalCharges) were capped using the IQR method to prevent extreme values from skewing model training.

## Data Transformation**
**Feature Engineering:** The customerID column was dropped as it's not a predictive feature.
**Encoding:** Binary categorical features (gender, Partner, Dependents, PhoneService, PaperlessBilling, Churn) were mapped to numerical (0/1). Other multi-class categorical features were one-hot encoded (pd.get_dummies with drop_first=True).
**Feature Scaling:** Numerical features (SeniorCitizen, tenure, MonthlyCharges, TotalCharges) were standardized using StandardScaler to ensure all features contribute equally to the model.

## Exploratory Data Analysis (EDA) Highlights
**Churn Distribution:** The dataset shows an imbalanced class distribution with approximately 26.43% of customers churning.
**Correlation Analysis:** Key features positively correlated with churn include InternetService_Fiber optic, PaymentMethod_Electronic check, MonthlyCharges, PaperlessBilling, and SeniorCitizen. Features negatively correlated (reducing churn likelihood) include tenure, Contract_Two year, OnlineSecurity_Yes, and TechSupport_Yes.
**Categorical & Numerical Analysis:** Visualizations confirmed these correlations, showing higher churn rates among customers with fiber optic internet, electronic check payment, month-to-month contracts, and shorter tenure.

## Feature Selection
**Recursive Feature Elimination (RFE):** RFE with a Logistic Regression estimator was used to select the most relevant features. 15 features were selected for model training, including tenure, TotalCharges, InternetService_Fiber optic, and various service and contract types.

## Model Building & Evaluation
The dataset was split into training (80%) and testing (20%) sets, stratified to maintain the churn class distribution:

| Model               | Accuracy | Precision | Recall | F1-Score | AUC      |
| :------------------ | :------- | :-------- | :----- | :------- | :------- |
| Logistic Regression | 0.7994   | 0.6467    | 0.5339 | 0.5849   | 0.8406   |
| Random Forest       | 0.7739   | 0.5915    | 0.4714 | 0.5246   | 0.8022   |
| SVC                 | 0.8036   | 0.6868    | 0.4740 | 0.5609   | 0.7890   |
| Decision Tree       | 0.7540   | 0.5405    | 0.4688 | 0.5021   | 0.7498   |

## Model Optimization

*   **Hyperparameter Tuning:** `GridSearchCV` was applied to the best-performing model, Logistic Regression, to optimize its hyperparameters. The tuning focused on `penalty`, `C`, and `solver` parameters, using 5-fold cross-validation and `roc_auc` as the scoring metric.
*   **Optimized Logistic Regression:** The optimized model achieved an improved cross-validation AUC score.

## Best Model

The **Optimized Logistic Regression** model emerged as the best performer:

*   **Best Parameters:** `C=100`, `penalty='l1'`, `solver='liblinear'`
*   **Best Cross-Validation AUC:** `0.8452`
*   **Performance on Test Set:**
    *   Accuracy: 0.8001
    *   Precision: 0.6478
    *   Recall: 0.5365
    *   F1-Score: 0.5869
    *   AUC: 0.8452

## Feature Importance & Business Insights (Key Finding)

Analyzing the coefficients of the Optimized Logistic Regression model and feature importances from Random Forest provided consistent insights:

**Current Churn Rate:** **26.43%.**
*   **Strongest Churn Drivers (Positive Coefficients/High Importance):**
    *   `InternetService_Fiber optic`
    *   `PaymentMethod_Electronic check`
    *   `MonthlyCharges`
    *   `PaperlessBilling`
    *   Lower `tenure` (due to negative coefficient)
*   **Strongest Retention Factors (Negative Coefficients/High Importance):**
    *   Higher `tenure`
    *   `Contract_Two year`
    *   `OnlineSecurity_Yes`
    *   `TechSupport_Yes`

These features consistently indicate that customers with shorter tenure, month-to-month contracts, fiber optic internet, and electronic check payments are at a higher risk of churning. Conversely, customers with long-term contracts and value-added services tend to be more loyal.

## Actionable Business Recommendations

Based on the analysis, here are actionable recommendations to reduce churn:

**🎯 IMMEDIATE ACTIONS:**

1.  **Enhance Customer Service for At-Risk Segments:**
    *   Proactively identify and monitor customers with characteristics of high churn risk (e.g., new customers, fiber optic users).
    *   Implement proactive outreach programs to offer personalized support or resolve potential issues before they escalate.
2.  **Optimize Contract and Payment Method Strategies:**
    *   Incentivize customers to switch from month-to-month to longer-term contracts (one or two years) with attractive discounts or bundled services.
    *   Encourage the adoption of more stable payment methods like credit card auto-pay or bank transfers over electronic checks, potentially offering small incentives.
3.  **Improve Fiber Optic Service and Perceptions:**
    *   Investigate customer satisfaction and common pain points specifically among Fiber Optic internet users. Address reliability, speed, or support issues.
    *   Highlight the benefits and value of Fiber Optic service more effectively to retain existing users.
4.  **Promote Value-Added Services:**
    *   Actively market and educate customers about the benefits of services like `OnlineSecurity` and `TechSupport`, especially to those currently without them. Consider free trials or bundled offers.

**📊 EXPECTED IMPACT:**

*   **Current Churn Rate:** 26.43%
*   **Model Recall:** The optimized Logistic Regression model can identify approximately **53.65%** of actual churners.
*   **Potential Revenue Impact:** By reducing churn, significant revenue can be saved. For instance, a **10% reduction in churn** could save **191 customers**, translating to an estimated **$95,500** in revenue (assuming $500/year per customer). A **20% reduction** could save **383 customers**, amounting to **$191,500** in revenue.

## Visualizations

The notebook includes:
-   Churn distribution analysis
-   Feature correlation heatmaps
-   Model performance comparisons
-   Feature importance rankings
-   ROC curves and confusion matrices

## Technologies Used

-   **Python 3.x**
-   **Data Analysis**: Pandas, NumPy
-   **Visualization**: Matplotlib, Seaborn
-   **Machine Learning**: Scikit-learn
-   **Model Persistence**: Joblib

## Methodology

1.  **Data Loading & Initial Exploration**
    *   Loading the `telco_churn.csv` dataset.
    *   Inspecting data types and summary statistics.
    *   Converting `TotalCharges` to numeric.
2.  **Data Preprocessing & Cleaning**
    *   Handling missing values using median imputation for numerical features and mode imputation for categorical features.
    *   Identifying and removing duplicate rows.
    *   Detecting and capping outliers in numerical features using the IQR method.
3.  **Data Transformation**
    *   Dropping irrelevant features (`customerID`).
    *   Encoding categorical variables (binary mapping for some, one-hot encoding for multi-class).
    *   Scaling numerical features using `StandardScaler`.
4.  **Exploratory Data Analysis (EDA)**
    *   Analyzing the distribution of the target variable (`Churn`).
    *   Visualizing feature correlation with a heatmap and bar charts.
    *   Analyzing churn rates across different categorical and numerical feature segments.
5.  **Feature Selection**
    *   Applying Recursive Feature Elimination (RFE) with Logistic Regression to select the most relevant features.
6.  **Model Building & Training**
    *   Splitting the dataset into training and testing sets (80/20 split) with stratification.
    *   Training multiple classification models: Logistic Regression, Random Forest, Decision Tree, and Support Vector Classifier (SVC).
7.  **Model Evaluation & Comparison**
    *   Evaluating models using key metrics: Accuracy, Precision, Recall, F1-Score, and AUC.
    *   Visualizing confusion matrices and ROC curves.
    *   Comparing the performance of all trained models.
8.  **Model Optimization**
    *   Performing hyperparameter tuning on the best-performing model (Logistic Regression) using `GridSearchCV` with 5-fold cross-validation, optimizing for AUC.
9.  **Feature Importance Analysis**
    *   Analyzing feature coefficients from the optimized Logistic Regression model.
    *   Extracting feature importances from the Random Forest model.
    *   Synthesizing insights from both models to identify key churn drivers and retention factors.
10. **Business Insights & Recommendations**
    *   Formulating actionable recommendations based on model findings and feature importances.
    *   Projecting potential business impact of churn reduction.
11. **Model Persistence**
    *   Saving the best model and preprocessing objects (`StandardScaler`, `trained_features`) for future use.
    *   Developing and demonstrating a `predict_churn` function for new data.
