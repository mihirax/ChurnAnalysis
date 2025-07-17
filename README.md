# ChurnCast — Churn Prediction & BI Dashboard

**ChurnCast** is an end-to-end churn intelligence system for telecom customer retention. It combines **machine learning** with **Power BI dashboards** to analyze, explain, and predict customer churn. Built on a real-world-like Telco dataset, it enables business users to **visualize churn risk**, understand driving factors, and identify segments for targeted interventions.

>  Dashboard-first, model-supported — built to showcase BI, CRM analytics, and actionable storytelling.

---

##  Table of Contents

* [Project Overview](#project-overview)
* [Directory Structure](#directory-structure)
* [Model Design](#model-design)
* [Data Pipeline](#data-pipeline)
* [Installation](#installation)
* [Usage](#usage)
* [Features](#features)
* [Evaluation](#evaluation)
* [Dashboard Overview](#dashboard-overview)
* [Business Insights & Recommendations](#business-insights--recommendations)
* [Acknowledgments](#acknowledgments)

---

## Project Overview

ChurnCast helps telecom providers:

* **Predict customer churn** with 90%+ accuracy (CatBoost)
* **Explain model decisions** using SHAP and regression coefficients
* **Visualize churn patterns** by demographics, contracts, internet type, tenure, and more
* **Empower CRM and BI teams** with actionable insights through a Power BI dashboard

It brings together machine learning modeling in Python and interactive dashboards in Power BI to simulate a real-world **CRM analytics system**.

---

## Directory Structure

```
ChurnCast/
├── app/
│   └── churn_dashboard.pbix        # Power BI file
├── data/
│   ├── cleaned_telco.csv           # Final cleaned dataset
│   └── raw_telco_sample.csv        # Sample of raw input data
├── model/
│   ├── catboost_model.pkl
│   ├── logistic_regression.pkl
│   └── shap_visuals.ipynb          # SHAP explanation code
├── notebooks/
│   └── churn_modeling.ipynb        # Full training notebook
├── visuals/
│   ├── churn_analysis.png
│   ├── churn_predictions.png
│   └── model_insights.png
├── requirements.txt
└── README.md
```

---

## Model Design

**Target:** Binary classification — `Churned` vs. `Stayed`
(`Joined` customers were excluded from training and used for prediction later.)

**Models Tried:**

* CatBoost (best ROC-AUC: 0.96)
* LightGBM, XGBoost, Random Forest, Logistic Regression
* Decision Tree, KNN, Naive Bayes, AdaBoost, SVM

**Final Choice:**

* **CatBoost** for prediction
* **Logistic Regression** for interpretability

**Handling Imbalance:**

* Used **SMOTE** from `imblearn` to balance churned/stayed classes

**Encoding:**

* Binary features: Label Encoding
* Categorical (multi-class): One-Hot Encoding

---

## Data Pipeline

| Step           | Action                                                                  |
| -------------- | ----------------------------------------------------------------------- |
| Missing Values | Imputed based on logical rules — e.g., 'No Internet' or 'None'          |
| Cleaning       | Removed irrelevant columns (ID, Churn\_Reason, Revenue during modeling) |
| Split          | 70/30 Train-Test with Stratification                                    |
| Resampling     | Applied SMOTE before model training                                     |
| Prediction     | Used model to predict churn on previously unseen “Joined” customers     |
| Visuals        | Created in Power BI (post-feature engineering)                          |

---

## Installation

```bash
git clone https://github.com/yourusername/ChurnCast.git
cd ChurnCast
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements.txt
```

---

## Usage

### Train the Model (Optional)

```python
# Inside churn_modeling.ipynb
Run all cells to preprocess data, apply SMOTE, train models, and evaluate.
```

### Explain Predictions with SHAP

```python
# Inside shap_visuals.ipynb
Explains CatBoost predictions with global and local SHAP plots.
```

### Open Dashboard

1. Open `app/churn_dashboard.pbix` in Power BI Desktop
2. Interact with slicers (state, gender, contract)
3. Analyze churn metrics, probabilities, and customer segments

---

## Features

* 🔍 **Prediction**

  * Detect high-risk churners (churn prob > 0.5)
  * Uses CatBoost with 96% AUC and 90% accuracy
* **Model Explainability**

  * SHAP visualizations for CatBoost
  * Coefficient bar chart for Logistic Regression
* **BI Dashboards**

  * CRM-style KPIs
  * Drilldowns by age, contract, internet type, and churn reason
  * Filtered customer tables with predicted probabilities

---

## Evaluation

| Metric              | CatBoost                                           |
| ------------------- | -------------------------------------------------- |
| ROC-AUC             | 0.96                                               |
| Accuracy            | 0.90                                               |
| SMOTE Used          | ✅                                                  |
| High-Risk Customers | Identified and filtered via probability thresholds |

> Feature Importance Highlights:
>
> * `Contract: Month-to-Month` (Top driver of churn)
> * `Internet Type: Fiber Optic`, `Device Protection`, `Age`

---

## Dashboard Overview

### 1. **Model Insights**

* Feature importance from CatBoost and Logistic Regression
* Comparative bar chart of all models’ ROC-AUC and Accuracy

### 2. **Churn Predictions**

* Count of predicted churners
* Breakdown by contract, age, gender
* Predicted churn probabilities (bins and bubble chart)
* Table with actual high-risk customers

### 3. **Churn Analysis**

* Churn rate by:

  * Age group
  * Contract type
  * State
  * Internet type
  * Payment method
* Most common churn reasons
* KPIs: Total Churn, New Joiners, Churn Rate (27%)

---

## Business Insights & Recommendations

### Key Findings from Dashboard

#### 1. **Contract Type is the Most Powerful Predictor**

* **Churn rate for Month-to-Month contracts is 46.5%**, significantly higher than other plans.
* Annual or 2-Year contracts show **better retention** and lower volatility.

#### 2. **Fiber Optic Users Churn the Most**

* Customers with **Fiber Optic internet** account for **41.1% of total churn**.
* DSL and Cable show lower churn rates, possibly due to lower expectations or pricing.

#### 3. **Age Factor**

* Customers aged **50+ show the highest churn rate (\~31.6%)**.
* Middle-aged groups (36–50) also contribute significantly.

#### 4. **Churn Reasons Show Device/Service Competition**

Top reasons for churn:

* **"Competitor had better devices"**
* **"Competitor offered better services"**
* **"Attitude of support staff"**

#### 5. **Tenure Paradox**

* While short-tenure customers do churn, **long-tenure customers (>24 months)** still contribute notable churn — loyalty isn't guaranteed without continued value.

---

### Actionable Recommendations

* **Incentivize Long-Term Plans**

  * Offer discounts or bundled deals to migrate Month-to-Month users to 1 or 2-year plans.

* **Audit Fiber Plans**

  * Investigate Fiber churn — is it pricing, service reliability, or device compatibility?

* **Loyalty Programs for Seniors**

  * Introduce age-specific offers, device support, or "senior-friendly" bundles.

* **Improve Customer Support**

  * Focus on **soft skills training** — support experience directly impacts churn.

* **Run Targeted Retention Campaigns**

  * Use churn predictions to trigger CRM actions before contracts expire or issues escalate.

---

## Acknowledgments

* [CatBoost](https://catboost.ai/)
* [SHAP](https://github.com/slundberg/shap) for explainable AI
* [Power BI](https://powerbi.microsoft.com/) for dashboarding
* [SMOTE - Imbalanced-learn](https://imbalanced-learn.org/) for resampling


