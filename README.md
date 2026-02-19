# Bank Telemarketing – Classifier Comparison

## 1. Business Objective

The objective is to improve the efficiency of the bank’s telemarketing campaigns for **term deposits** by predicting which clients are likely to subscribe, so that managers can prioritize calls and reduce total contacts while maintaining or increasing successful subscriptions.

---

## 2. Data and Features

- Source: Portuguese bank telemarketing data (`bank-additional-full.csv`) from the UCI “Bank Marketing (with social/economic context)” dataset and related CRISP‑DM study. 
- Size: 41,188 records, 20 input features + binary target `y` (subscribe: `"yes"` / `"no"`).  
- Class imbalance: only about 11–12% `"yes"`, 88–89% `"no"`.

For this assignment, I used only **bank client features**:

- `age` (numeric)  
- `job`, `marital`, `education`, `default`, `housing`, `loan` (categorical)

Target encoding:

- `y`: mapped to 1 (`"yes"`) and 0 (`"no"`).

---

## 3. Preparation and Baseline

Key steps:

- Load data with `sep=";"`.  
- Select bank client features and target.  
- One‑hot encode categorical variables (`get_dummies`, keeping `"unknown"` as a category).
- Train/test split: 70% train, 30% test, stratified by `y`.

Baseline:

- A classifier that always predicts `"no"` achieves about **88–89% accuracy**, due to class imbalance; all models should beat this and improve recall for `y=1`.

---

## 4. Models and Metrics

Models (default settings, except `max_iter` for Logistic Regression):

- Logistic Regression  
- k-Nearest Neighbors (KNN)  
- Decision Tree  
- Support Vector Machine (SVM)

For each model, I recorded:

- Train time  
- Train accuracy  
- Test accuracy

Example comparison table:

| Model              | Train Time (s) | Train Accuracy | Test Accuracy |
|--------------------|----------------|----------------|---------------|
| Logistic Regression| 1.6489         | 0.8873         | 0.8874        |
| KNN                | 0.0182         | 0.8891         | 0.8738        |
| Decision Tree      | 0.1195         | 0.9188         | 0.8639        |
| SVM                | 16.6516        | 0.8873         | 0.8874        |

Because of class imbalance, I also examined **precision, recall, and F1** for the positive class, and (optionally) ROC AUC, which better reflect how well the model identifies likely subscribers.

---

## 5. Improvements and Insights

To improve performance, I:

- Tuned hyperparameters using `GridSearchCV` (e.g., `n_neighbors` for KNN, `max_depth` for Decision Tree.
- Focused on models that improved **F1 and recall** for `y=1`, not just accuracy.

Typical pattern from this dataset and literature:

- Logistic Regression and SVM tend to offer strong, stable performance on one‑hot encoded features.
- Decision Trees can overfit without constraints but are interpretable.  
- KNN performance depends on `n_neighbors` and can be weaker in high dimensions.

**Recommendation:** Use the best‑performing tuned model (often Logistic Regression or SVM) to score clients before campaigns and rank them by predicted probability of subscription, so the bank can focus calls on high‑priority segments and monitor performance over time.
```
