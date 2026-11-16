# SI

Lab 1: Exploratory Data Analysis (EDA), Parameterized Logistic Regression & Explainable AI (SHAP) for Phishing Detection

## 📌 Project Overview

This project focuses on detecting phishing websites using Machine Learning and Explainable AI (XAI). It combines Exploratory Data Analysis (EDA), Logistic Regression, and SHAP to identify suspicious URLs and interpret model predictions.

The objective is to develop an effective and interpretable phishing detection approach suitable for cybersecurity applications and Security Operations Centers (SOC).

## 🎯 Objectives

- Analyze and understand the PhiUSIIL phishing URL dataset.
- Perform data cleaning and Exploratory Data Analysis (EDA).
- Apply preprocessing and feature standardization.
- Train and compare different Logistic Regression configurations.
- Evaluate model performance using classification metrics.
- Interpret predictions using SHAP.
- Analyze decision thresholds for SOC operations.




## 🔬 Laboratory Workflow

**Part 1 – Exploratory Data Analysis (EDA)**
- Dataset inspection and data quality analysis.
- Class distribution and feature analysis.
- Feature selection and correlation analysis.

**Part 2 – Data Preprocessing**
- Stratified train-test splitting (75% / 25%).
- Standardization using StandardScaler.
- Prevention of data leakage.

**Part 3 – Logistic Regression**
- Comparison of five regularization configurations.
- L1 and L2 regularization.
- Feature selection and Odds Ratio interpretation.
- Model performance evaluation.

**Part 4 – Explainable AI (SHAP)**
- Global feature importance.
- Local prediction explanations.
- SHAP waterfall visualizations.
- Decision threshold analysis for SOC operations.

## 📁 Dataset

**Dataset:** PhiUSIIL Phishing URL Dataset.

The dataset contains URL-related and website-based features used to distinguish between legitimate and phishing websites.

## 🔗 Project Links

- **Kaggle Notebook:** [View Project on Kaggle](https://www.kaggle.com/code/marwalaaouichi/notebook85d8537cbe)


*This project was developed as part of a laboratory assignment in Information Systems, focusing on Machine Learning applications in cybersecurity.*


---

## Lab 2 – Static Malware Detection on Windows PE Files

### Objective

Build and compare machine-learning detectors for Windows Portable Executable (PE) files in a fictional case study (**AtlasPay Services**), select a decision threshold under explicit SOC costs, and deliver a defensible recommendation to the CISO. The task is binary classification (positive class = malware), deployed first as **analyst-assisted triage**, not automatic blocking.

### Methodology

1. **Data audit:** removed the identifier (`SHA256_File_ID`) and the post-scan field (`VT_PostScan_Detections`, target leakage); replaced 59 corrupted values (`99999.0`) with `NaN`.
2. **Evaluation protocol:** stratified 80/20 development/test split (lockbox test set), 5-fold stratified cross-validation and leakage-free `Pipeline` preprocessing.
3. **Benchmark:** Logistic Regression, Decision Tree, Random Forest, LightGBM.
4. **Explainability:** error analysis of a verified model disagreement using TreeSHAP.
5. **Cost optimization:** threshold selected on out-of-fold development data (FP = $25, FN = $500), frozen, then evaluated once on the test set.

### Key results

| Model | Validation F1 | Validation PR-AUC | Latency (ms / 1,000 files) |
|---|---|---|---|
| Logistic Regression | 0.9238 | 0.9080 | 6.0 |
| LightGBM | 0.9229 | 0.9080 | 13.1 |
| Random Forest | 0.9237 | 0.9049 | 48.3 |
| Decision Tree | 0.8301 | 0.7330 | – |

Test set at the frozen threshold (Logistic Regression, 0.2873): **10 FP, 34 FN, precision 0.9664, recall 0.8944, cost $17,250**.

### Recommendation

- **Model:** Logistic Regression (competitive PR-AUC, no overfitting, fastest, simplest to deploy); LightGBM kept as a challenger.
- **Threshold:** 0.2873, selected on development data only.
- **Alert volume:** not compatible with the 120-alert/day target (about 2,980 per 12,000 files, indicative only). Real telemetry and calibration are required for a reliable forecast.
- **Inline blocking:** not recommended before validation on real data.

### Limitations

Synthetic data, probably unrepresentative prevalence (26.8%), noisy labels, calibration not verified, and drift and adversarial adaptation not tested.

