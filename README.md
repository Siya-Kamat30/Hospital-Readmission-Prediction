# 🏥 Hospital Readmission Prediction — Diabetes Patients

Predicting 30-day hospital readmission risk for diabetic 
patients using machine learning, with model comparison, 
hyperparameter tuning and actionable insights for hospital 
risk-flagging systems.

![Python](https://img.shields.io/badge/Python-3.8+-blue)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-orange)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 📋 Project Overview

Hospital readmissions within 30 days represent a 
significant quality and cost challenge in healthcare. 
This project builds an end-to-end ML pipeline to 
identify high-risk diabetic patients using real 
clinical data from 130 US hospitals.

**Business Question:**
Which diabetic patients are most likely to be 
readmitted within 30 days and what clinical 
factors are associated with that risk?

**Reference:** This project was built following the 
approach of 
[Andrew Long's diabetes readmission analysis](https://github.com/andrewwlong/diabetes_readmission), 
with additional feature importance analysis and 
clinical insights developed independently.

---

## 📊 Dataset

| Property | Details |
|----------|---------|
| Source | UCI ML Repository — Diabetes 130-US Hospitals |
| Records | 101,766 patient encounters (99,343 after cleaning) |
| Features | 50 original variables → 143 model features |
| Target | 30-day readmission (binary) |
| Class balance | ~11% positive (readmitted <30 days) |
| Time period | 1999–2008, 130 US hospitals |

**Key variables:** patient demographics, diagnoses, 
medications, lab results, HbA1c measurements, 
prior visit history

---

## 🔧 Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3.8+ | Core language |
| Pandas & NumPy | Data cleaning and transformation |
| Scikit-learn | ML modeling and evaluation |
| Matplotlib & Seaborn | EDA and visualization |
| Jupyter Notebook | Development environment |

---

## 🚀 Project Pipeline

```
Raw Data (101,766 records)
        ↓
Data Cleaning
(remove death/hospice discharges, replace ? with NaN,
fill missing categories with "UNK")
        ↓
Exploratory Data Analysis
(distributions, class imbalance)
        ↓
Feature Engineering
(one-hot encoding, top-10 medical specialties,
age group, has-weight flag, scaling)
        ↓
Train / Validation / Test Split (70 / 15 / 15)
(training set balanced by undersampling)
        ↓
Model Comparison (7 models) + Hyperparameter Tuning
(KNN, Logistic Regression, SGD, Naive Bayes,
Decision Tree, Random Forest, Gradient Boosting)
        ↓
Final Evaluation on Held-Out Test Set
(AUC, accuracy, recall, precision, specificity)
        ↓
Feature Importance & Clinical Insights
(risk-flagging criteria for hospitals)
```

---

## 🧹 Data Cleaning

| Step | Action |
|------|--------|
| Non-eligible patients | Removed encounters ending in death or hospice (they cannot be readmitted) → 99,343 encounters |
| Missing values | Replaced `?` with `NaN`; filled `race`, `max_glu_serum`, `A1Cresult`, `payer_code`, `medical_specialty` with an "UNK" category |
| Medical specialty | Kept the top 10 values, grouped the rest as "Other" |
| Dropped columns | Identifiers, `diag_1/2/3` (hundreds of codes; `number_diagnoses` kept), single-value columns (`examide`, `citoglipton`) |
| Target encoding | `<30` days → 1, else → 0 |

---

## ⚙️ Feature Engineering

| Feature | Description |
|---------|-------------|
| One-hot encoding | Categorical variables (race, gender, medications, A1C result, admission/discharge IDs, payer code, specialty), `drop_first=True` |
| `age_group` | Age bracket mapped to a number (0, 10, … 90) |
| `has_weight` | Whether a weight value was recorded |
| Scaling | `StandardScaler` fitted on the training split only |

Total: **143 features** (8 numeric, 133 encoded, 2 extra).

---

## 🔀 Train / Validation / Test Split

- 70% training, 15% validation, 15% test (random, `random_state=42`)
- Training set **balanced by undersampling** the majority class (15,766 rows, 50% readmitted)
- Validation and test sets keep the real class ratio (~11%)
- Models chosen on validation data; test set used once at the end

---

## 🤖 Model Results

| Model | Validation AUC |
|-------|----------------|
| K-Nearest Neighbours | 0.621 |
| Logistic Regression | 0.660 |
| SGD Classifier | 0.661 |
| Naive Bayes | 0.505 |
| Decision Tree | 0.623 |
| Random Forest | 0.655 (tuned: 0.662) |
| Gradient Boosting | 0.639 (tuned: **0.669**) |

Hyperparameters tuned with `RandomizedSearchCV` (20 candidates, 2-fold CV, AUC).

**Final model: tuned Gradient Boosting** (100 trees, max depth 3, learning rate 0.1)

| Set | AUC | Accuracy | Recall | Precision | Specificity |
|-----|-----|----------|--------|-----------|-------------|
| Validation | 0.669 | 0.661 | 0.574 | 0.182 | 0.672 |
| **Test** | **0.666** | 0.653 | **0.576** | **0.185** | 0.664 |

> **Note:** AUC is the primary metric as it better captures 
> model discrimination on imbalanced healthcare data than 
> accuracy alone. The model catches ~58% of readmissions. 
> Precision is low because only ~11.7% of patients are 
> readmitted; it is about 1.6× better than random selection. 
> Published AUCs on this dataset are in a similar range 
> (roughly 0.63–0.70).

---

## 📈 Feature Importance Analysis

Top features in the Random Forest model:

| Rank | Feature |
|------|---------|
| 1 | Number of prior inpatient visits |
| 2 | Number of emergency visits |
| 3 | Discharge disposition (ID 22) |
| 4 | Number of medications |
| 5 | Time in hospital |

Prior inpatient visits was also the top feature in the 
Logistic Regression model. Feature importance shows what 
the model uses to predict, not what causes readmission.

---

## 💡 Key Clinical Insights

- **Prior inpatient visits:** readmitted patients averaged 
  1.22 prior inpatient visits vs 0.55 for others — the 
  strongest signal

- **HbA1c not measured:** readmission rate was ~11.7% when 
  no HbA1c result was recorded, vs ~9.8–10.1% when tested

- **Age:** highest in the 20–30 group (14.3%, a small group), 
  then 80–90 (12.6%) and 70–80 (12.1%)

- **Medications:** readmitted patients averaged slightly 
  more medications (16.9 vs 15.9)

> These are associations, not causes. A risk-flagging 
> system could prioritize patients with prior inpatient 
> admissions, older patients, and those without recent 
> HbA1c monitoring.

---

## ⚠️ Limitations

- Split by encounter, not by patient, so the same patient 
  can appear in both training and test
- Modest performance (AUC ~0.67) and low precision
- Data from 1999–2008, US hospitals only
- Undersampling discards majority-class data; predicted 
  probabilities are not calibrated to the real ~11% rate
- Light hyperparameter tuning (2-fold CV)

---

## 🔮 Future Work

- [ ] Split by patient and use 5-fold cross-validation
- [ ] Add XGBoost model comparison
- [ ] Group diagnosis codes into clinical categories
- [ ] Build Tableau dashboard visualizing risk 
      segments and clinical KPIs
- [ ] Deploy as simple Flask web app for 
      risk scoring
- [ ] Incorporate MIMIC-IV dataset for 
      more recent data

---

## 📚 References

- [UCI ML Repository — Diabetes 130-US Hospitals Dataset](https://archive.ics.uci.edu/ml/datasets/Diabetes+130-US+hospitals+for+years+1999-2008)
- [Andrew Long — Diabetes Readmission Analysis](https://github.com/andrewwlong/diabetes_readmission)
- Strack et al. (2014) — Impact of HbA1c Measurement on Hospital Readmission Rates

---

## 👩‍💻 Author

**Siya Kamat**  
MSc Web and Data Science, University of Koblenz  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-siya--kamat30-blue)](https://linkedin.com/in/siya-kamat30)
[![GitHub](https://img.shields.io/badge/GitHub-Siya--Kamat30-black)](https://github.com/Siya-Kamat30)
