# Diabetes Risk Classification Using Machine Learning

Machine-learning classification of diabetes status from routinely collected health and lifestyle indicators, comparing an interpretable linear model with a tree-based ensemble.

---

## Overview

This project investigates whether survey-based health and lifestyle indicators can be used to classify diabetes status. Two supervised classification models were trained and compared:

- **Logistic Regression** (with feature scaling), used as an interpretable baseline
- **Random Forest Classifier**, used as a non-linear ensemble model

Models were evaluated on a held-out test set and with five-fold stratified cross-validation, and Random Forest feature importance was used to examine which indicators the model relied on most.

---

## Objective

- Classify individuals as diabetic or non-diabetic using health and lifestyle indicators
- Compare a linear model with a non-linear ensemble model
- Evaluate models with multiple metrics, not accuracy alone
- Examine which features contribute most to model predictions

---

## Dataset

**CDC Diabetes Health Indicators (BRFSS 2015)**, obtained from Kaggle (Diabetes Health Indicators Dataset by Alex Teboul), derived from the U.S. Behavioral Risk Factor Surveillance System survey.

The balanced version of the dataset (`diabetes_binary_5050split_health_indicators_BRFSS2015.csv`) was used:

| Property | Value |
|---|---|
| Rows | 70,692 |
| Input features | 21 |
| Target | `Diabetes_binary` (0 = no diabetes, 1 = diabetes) |
| Class balance | 35,346 / 35,346 (50:50) |
| Missing values | 0 |

**Features include:** HighBP, HighChol, CholCheck, BMI, Smoker, Stroke, HeartDiseaseorAttack, PhysActivity, Fruits, Veggies, HvyAlcoholConsump, AnyHealthcare, NoDocbcCost, GenHlth, MentHlth, PhysHlth, DiffWalk, Sex, Age, Education and Income.

---

## Workflow

```
Data loading
  → Data checks (missing values, duplicates, class balance)
  → Exploratory analysis (class distribution, BMI by diabetes status)
  → Stratified train/test split (80:20)
  → Logistic Regression (StandardScaler + LogisticRegression pipeline)
  → Random Forest (300 trees)
  → Test-set evaluation
  → Five-fold stratified cross-validation (ROC-AUC)
  → ROC curves and confusion matrix
  → Feature-importance analysis
```

---

## Results

### Test-set performance

| Model | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---|---|---|---|---|
| Logistic Regression | 0.746 | 0.739 | 0.760 | 0.750 | **0.827** |
| Random Forest | 0.734 | 0.719 | **0.769** | 0.743 | 0.811 |

### Five-fold stratified cross-validation (training set)

| Model | Mean ROC-AUC | Std. deviation |
|---|---|---|
| Logistic Regression | 0.821 | ± 0.005 |
| Random Forest | 0.810 | ± 0.004 |

Logistic Regression performed slightly better overall (accuracy, F1-score and ROC-AUC), while Random Forest achieved slightly higher recall. The low cross-validation standard deviation indicates stable performance across folds.

In a screening context, **recall** is particularly important, because failing to identify a person with diabetes (a false negative) is generally more costly than a false alarm.

### Feature importance (Random Forest)

The most influential features were:

1. BMI
2. Age
3. General health (GenHlth)
4. Income
5. High blood pressure (HighBP)

These values describe how the model used each variable and **should not be interpreted as evidence that these variables cause diabetes.**

---

## Visualizations

- Diabetes status distribution
- BMI by diabetes status (boxplot)
- Random Forest confusion matrix
- ROC curves for both models
- Top 10 feature importances

---

## Tools and Technologies

- Python
- pandas, NumPy
- scikit-learn (Pipeline, StandardScaler, LogisticRegression, RandomForestClassifier, StratifiedKFold)
- Matplotlib
- Kaggle Notebooks

---

## Files in This Repository

- `diabetes_risk_classification.ipynb` – main notebook
- `README.md`

---

## Limitations

- The data are survey-based and observational, so results show **associations, not causation**.
- The balanced 50:50 dataset does not reflect the real-world prevalence of diabetes, so performance on real populations would differ.
- Several variables are self-reported and may contain reporting bias.
- Duplicate rows were identified but not removed.
- Model performance was not compared across demographic subgroups (e.g., age or sex).

---

## Future Improvements

- Evaluate on the original imbalanced dataset using class weights or resampling
- Add gradient-boosting models (e.g., XGBoost)
- Tune the decision threshold to prioritise recall for screening
- Use SHAP for model explainability at the individual level
- Assess fairness by comparing performance across age groups and sex

---

## Disclaimer

This project was developed for educational and portfolio purposes only and is **not a medical diagnostic tool**.
