# Waze User Churn Analysis and Prediction

## Executive Summary

This project analyzes monthly user churn for the Waze navigation app and evaluates whether user behavior data can be used to identify users at risk of leaving the platform.

The analysis follows an end-to-end analytics workflow, including exploratory data analysis, statistical testing, feature engineering, and predictive modeling using both traditional statistics and machine learning.

Key outcome:  
Advanced models improved churn detection compared to a statistical baseline, but results show that additional and more granular data would be required for reliable churn prediction in production.

---

## Business Context

Waze depends on active users to maintain accurate, real-time navigation data. User churn reduces both engagement and the quality of crowd-sourced insights.

Churn in this project is defined as users who stopped using or uninstalled the app within a single month. The objective is to understand which usage patterns correlate with churn and to assess whether churn can be predicted early enough to support retention strategies.

---

## What This Project Demonstrates

- Translating a business problem into an analytics problem
- Cleaning and validating real-world behavioral data
- Performing exploratory data analysis to surface risk signals
- Applying hypothesis testing correctly
- Engineering features to improve model signal
- Evaluating classification models using business-relevant metrics
- Communicating insights clearly for non-technical stakeholders

---

## Data Overview

Each row represents one user’s activity during a single month.

Key variables include:
- Number of sessions
- Number of drives
- Kilometers driven
- Drive duration in minutes
- Activity days and driving days
- Device type
- Churn label

### Class Balance and Device Distribution

| User churn | Device usage |
|-----------|--------------|
| ![Churn distribution](images/churn_distribution.png) | ![Device distribution](images/device_distribution.png) |

Most users were retained during the month, creating a class imbalance that increases the difficulty of churn prediction. Device usage is skewed toward iPhone, but device type alone does not explain churn.

---

## Exploratory Data Analysis

User engagement varies significantly, with strong right-skew and extreme outliers across multiple metrics.

### Session Activity

| Session distribution | Session outliers |
|---------------------|------------------|
| ![Sessions histogram](images/sessions_histogram.png) | ![Sessions boxplot](images/sessions_boxplot.png) |

Most users cluster around the median number of sessions, but a small group of high-usage users forms a long tail. These users may represent power users, professional drivers, or data anomalies.

### Driving Distance

![Kilometers driven histogram](images/kilometers_histogram.png)

Driving distance also shows wide variability, suggesting distinct usage profiles across the user base.

### Feature Relationships

![Driving days vs activity days](images/driving_days_vs_activity_days.png)

Driving days and activity days are highly correlated, indicating redundant information that must be handled carefully during modeling.

---

## Hypothesis Testing

A two-sample t-test was conducted to evaluate whether average monthly drives differ between iPhone and Android users.

Results:
- iPhone mean drives: 67.86
- Android mean drives: 66.23
- p-value: 0.143

The null hypothesis was not rejected. Device type does not meaningfully explain usage differences or churn risk.

---

## Baseline Model: Logistic Regression

A binomial logistic regression model was built as a baseline predictor.

Additional engineered features included:
- Kilometers per driving day
- Professional driver indicator (more than 60 drives per month)

![Correlation heatmap](images/correlation_heatmap.png)

Strong correlations between several features required removal of redundant variables prior to modeling.

Baseline model performance:
- Accuracy: 0.82
- Precision: 0.52
- Recall: 0.09
- F1 score: 0.16

The low recall indicates that many churned users were missed, limiting the model’s usefulness for proactive retention.

![Logistic regression confusion matrix](images/logreg_confusion_matrix.png)

---

## Advanced Models: Random Forest and XGBoost

Tree-based models were trained to capture non-linear relationships and feature interactions.

### Model Comparison

| Logistic regression | XGBoost |
|--------------------|---------|
| ![Logistic regression confusion matrix](images/logreg_confusion_matrix.png) | ![XGBoost confusion matrix](images/xgb_confusion_matrix.png) |

XGBoost was selected as the champion model due to improved recall while maintaining reasonable precision.

Final XGBoost performance:
- Accuracy: 0.81
- Precision: 0.39
- Recall: 0.17
- F1 score: 0.23

Although improved, performance remains insufficient for high-confidence churn prediction.

---

## Feature Importance

![XGBoost feature importance](images/xgb_feature_importance.png)

Most of the strongest predictors were engineered features rather than raw inputs, highlighting the importance of feature engineering. Behavioral intensity and recency signals were more informative than simple usage counts.

---

## Interpretation and Limitations

While machine learning models improved churn detection relative to a baseline, performance is constrained by data granularity. Aggregated monthly metrics do not fully capture behavioral intent or user satisfaction.

More detailed temporal and contextual data would likely improve predictive performance.

---

## Recommendations

- Collect more granular behavioral and temporal data
- Expand feature engineering focused on engagement patterns
- Analyze high-distance and irregular-use users separately
- Maintain parity between Android and iPhone experiences
- Treat churn modeling as an iterative, continuously improved process

---

## Repository Structure

- `notebooks/` – analysis notebooks in execution order
- `images/` – figures used in this README
- `reports/` – summaries and supporting material

---

## Final Note

This project was completed as part of the Google Advanced Data Analytics Certificate and reflects applied skills in data analysis, statistics, and machine learning using a realistic business scenario.
