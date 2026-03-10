# Stroke Prediction Dataset — Exploratory Data Analysis

> Healthcare Analytics Project | Python · Pandas · Seaborn · Matplotlib

---

## Project Overview

This project performs a comprehensive Exploratory Data Analysis (EDA) on the Healthcare Stroke Dataset to identify key risk factors associated with stroke incidence. The analysis covers data cleaning, feature engineering, statistical exploration, and multivariate visual analysis.

---

## Dataset

| Attribute | Detail |
|---|---|
| **Source** | Healthcare Stroke Dataset (CSV) |
| **Records** | 5,110 (5,109 after cleaning) |
| **Features** | 12 original columns (13 after engineering) |
| **Target Variable** | `stroke` (binary: 0 = No Stroke, 1 = Stroke) |
| **Class Imbalance** | ~95.1% No Stroke vs ~4.9% Stroke |

---

## Feature Descriptions

| Column | Type | Description |
|---|---|---|
| `id` | Integer | Unique patient identifier *(dropped)* |
| `gender` | Categorical | Male / Female / Other |
| `age` | Float → Integer | Patient age in years |
| `hypertension` | Binary | 1 if patient has hypertension |
| `heart_disease` | Binary | 1 if patient has heart disease |
| `ever_married` | Binary (encoded) | 1 = Yes, 0 = No |
| `work_type` | Categorical | Private / Self-employed / Govt_job / children / Never_worked |
| `Residence_type` | Categorical | Urban or Rural |
| `avg_glucose_level` | Float | Average blood glucose level |
| `bmi` | Float | Body Mass Index *(201 missing values imputed)* |
| `smoking_status` | Categorical | Never smoked / formerly smoked / smokes / Unknown |
| `stroke` | Binary *(Target)* | 1 = Stroke occurred |
| `agegroup` *(engineered)* | Categorical | Teens / Young adult / Early midlife / Late midlife / Early Elderly / Old age |

---

## Data Cleaning & Preprocessing

### Missing Values
- Only `bmi` had missing values: **201 rows (~3.93%)**
- **Strategy:** Median imputation grouped by engineered age group to account for outliers and the age–BMI relationship

| Age Group | Median BMI |
|---|---|
| Teens | 19.90 |
| Young Adult | 27.70 |
| Early Midlife | 30.30 |
| Late Midlife | 30.40 |
| Early Elderly | 29.55 |
| Old Age | 28.00 |

### Outlier Handling
- `bmi`: Right-skewed with outliers — median imputation used (robust to outliers)
- `avg_glucose_level`: Right-skewed distribution — retained as-is for modelling

### Encoding & Cleaning
- `ever_married`: Yes → 1, No → 0
- `age`: Converted from float to integer
- `gender`: Removed single `'Other'` record (index 3116) — statistically insignificant (1 row)
- `id`: Dropped — high cardinality, non-predictive

---

## EDA Key Findings

### 1. Target Variable Imbalance
- 95.1% of patients have no stroke; only **4.9%** experienced a stroke
- Imbalanced target — requires techniques like **SMOTE** or class weighting during modelling

### 2. Age & Stroke
- Stroke patients are significantly older (mean age: **67.7**) vs non-stroke (mean: **42.0**)
- Stroke incidence near **zero below age 40**
- Sharp rise after age **55–60**; highest rates in individuals aged **70+**
- Rare pediatric cases exist (ages 1 and 14)

### 3. Heart Disease
- Individuals with heart disease have **4× higher** stroke incidence
- Stroke rate: **17.0%** (heart disease) vs **4.2%** (no heart disease)
- **Strongest binary predictor** identified in EDA

### 4. Average Glucose Level
- Stroke patients have significantly higher glucose (mean: **132.5** vs **104.8**)
- Wide upper IQR in stroke group — hyperglycemia strongly associated with stroke

### 5. Hypertension
- Hypertension prevalence rises sharply with age (from ~0.1% in Teens to ~23.9% in Old Age)
- Hypertensive patients in **Urban areas** show slightly higher stroke incidence than Rural

### 6. BMI
- Stroke patients have slightly higher BMI (median: **29.55** vs **28.00**)
- Distributions overlap substantially — BMI alone is a **weak predictor**
- Likely interacts with age and cardiovascular conditions

### 7. Work Type
- Self-employed: highest stroke rate (**7.9%**)
- Never_worked: **0%** stroke rate
- Children: near-zero stroke rate

### 8. Smoking Status
- Former smokers show the highest stroke rate (**7.9%**) — likely age-confounded
- Current smokers: **5.3%** | Never smoked: **4.8%**
- Smoking is associated with elevated risk but is a **secondary predictor**

### 9. Gender
- Stroke incidence slightly higher in males, but the gap is **marginal**
- Gender is a **weak predictor**

### 10. Correlation Analysis
- No strong multicollinearity detected among numerical features
- `bmi` shows moderate correlation with `ever_married` (r = 0.34) — likely age-mediated
- `avg_glucose_level` and `stroke` show the most meaningful numerical correlation

---

## Feature Importance Summary *(EDA-based)*

| Priority | Feature | Relationship to Stroke |
|---|---|---|
| 🔴 Strong | `age` | Non-linear; sharp rise after 60 |
| 🔴 Strong | `heart_disease` | 4× higher stroke rate |
| 🔴 Strong | `avg_glucose_level` | Mean difference of ~28 units; hyperglycemia key |
| 🔴 Strong | `hypertension` | Elevated risk, amplified in urban patients |
| 🟡 Moderate | `bmi` | Modest elevation in stroke patients |
| 🟡 Moderate | `smoking_status` | Former smokers highest risk (age confound) |
| 🟢 Weak | `gender` | Marginal male–female difference |
| 🟢 Weak | `work_type` | Self-employed slightly higher; likely lifestyle proxy |

---

## Technology Stack

| Library | Purpose |
|---|---|
| `pandas` | Data loading, manipulation, and groupby operations |
| `numpy` | Numerical computations |
| `matplotlib` | Line plots, bar charts, and custom visualisations |
| `seaborn` | Statistical plots (boxplot, histplot, heatmap, countplot) |

---

## Conclusions

Stroke incidence in this dataset is primarily driven by **clinical and age-related factors** rather than lifestyle variables alone:

- Stroke risk is predominantly **age-driven** with a non-linear acceleration after 55–60
- **Cardiovascular disease** (heart disease + hypertension) significantly amplifies risk
- **Elevated glucose levels** represent a strong biochemical marker for stroke
- Lifestyle factors (BMI, smoking) contribute **secondary risk**, often interacting with age
- Class imbalance (~95:5) must be addressed in any downstream predictive modelling

---

*Healthcare Stroke EDA — Analysis Report*
