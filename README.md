# 🏥 Hospital Readmission Prediction — Diabetes Patients

Predicting 30-day hospital readmission risk for diabetic 
patients using machine learning, with clinical feature 
engineering and actionable insights for hospital 
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
readmitted within 30 days — and what clinical 
factors drive that risk?

**Reference:** This project was built following the 
approach of 
[Andrew Long's diabetes readmission analysis](https://github.com/andrewwlong/diabetes_readmission), 
with additional Feature Importance Analysis and 
Clinical Insights developed independently.

---

## 📊 Dataset

| Property | Details |
|----------|---------|
| Source | UCI ML Repository — Diabetes 130-US Hospitals |
| Records | 101,766 patient encounters |
| Features | 50 clinical variables |
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
Raw Data (101,766 records)
        ↓
Data Cleaning
(replace ? with NaN, drop high-missing 
columns, remove duplicates)
        ↓
Exploratory Data Analysis
(distributions, class imbalance, 
correlations, readmission patterns)
        ↓
Feature Engineering
(age encoding, diagnosis grouping,
HbA1c status flag, medication change count)
        ↓
Model Training
(Logistic Regression + Random Forest
with class_weight='balanced')
        ↓
Model Evaluation
(AUC, F1-score, Precision, Recall,
Confusion Matrix)
        ↓
Feature Importance Analysis
(Random Forest feature rankings)
        ↓
Clinical Insights & Recommendations
(risk-flagging criteria for hospitals)

---

## 🧹 Data Cleaning

| Step | Action |
|------|--------|
| Missing values | Replaced `?` with `NaN` |
| High-missing columns | Dropped `weight` (97%), `payer_code` (40%), `medical_specialty` (49%) |
| Duplicates | Kept first encounter per patient |
| Invalid entries | Removed 3 records with invalid gender |
| Target encoding | `<30` days → 1, else → 0 |
| Final dataset | ~71,500 unique patient encounters |

---

## ⚙️ Feature Engineering

| Feature | Description |
|---------|-------------|
| `age_numeric` | Age group midpoint (e.g. [70-80) → 75) |
| `diag_1_group` | Primary diagnosis grouped into clinical categories (Circulatory, Respiratory, Digestive, Diabetes, Other) |
| `num_meds_changed` | Count of medication dosage changes during visit |
| `HbA1c_tested` | Binary flag — whether HbA1c was measured |

---

## 🤖 Model Results

| Model | Accuracy | AUC | Precision | Recall | F1 |
|-------|----------|-----|-----------|--------|----|
| Logistic Regression | 0.61 | 0.65 | 0.58 | 0.63 | 0.60 |
| Random Forest | 0.64 | 0.68 | 0.61 | 0.64 | 0.62 |

> **Note:** Class imbalance handled using 
> `class_weight='balanced'`. AUC is the primary 
> metric as it better captures model discrimination 
> on imbalanced healthcare data than accuracy alone.
> Realistic AUC scores for this dataset range 
> 0.63–0.70 across published studies.

---

## 📈 Feature Importance Analysis

Top predictors of 30-day readmission 
(Random Forest feature importance):

| Rank | Feature | Insight |
|------|---------|---------|
| 1 | Number of prior inpatient visits | Strongest single predictor — past behavior predicts future risk |
| 2 | Number of diagnoses | Higher clinical complexity = higher risk |
| 3 | Number of medications | Readmitted patients on significantly more medications |
| 4 | Time in hospital | Longer stays correlated with readmission |
| 5 | HbA1c not measured | Missing glucose monitoring signals higher risk |
| 6 | Age (70-90 group) | Elderly patients consistently higher probability |
| 7 | Primary diagnosis | Circulatory and respiratory highest readmission rates |

---

## 💡 Key Clinical Insights

- **HbA1c not measured:** Patients without HbA1c 
  testing showed higher readmission rates — 
  suggesting glucose monitoring gaps increase risk

- **Age:** Older age groups ([70-80), [80-90)) 
  consistently showed higher readmission probability 
  across all models

- **Medications:** Readmitted patients were on 
  average prescribed significantly more medications, 
  indicating higher clinical complexity

- **Prior inpatient visits:** Patients with prior 
  hospital visits were far more likely to be 
  readmitted — past behavior is the strongest 
  predictive signal

- **Diagnosis:** Circulatory and respiratory primary 
  diagnoses had the highest readmission rates among 
  all diagnosis groups

> **Clinical Recommendation:** A hospital 
> risk-flagging system should prioritize: elderly 
> patients (70+), those with prior inpatient 
> admissions, patients without recent HbA1c 
> monitoring, and those with circulatory or 
> respiratory primary diagnoses.

---

---

## 🔮 Future Work

- [ ] Build Tableau dashboard visualizing risk 
      segments and clinical KPIs
- [ ] Add XGBoost model comparison
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
