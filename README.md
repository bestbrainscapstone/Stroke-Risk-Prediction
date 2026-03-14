# Stroke Prediction — Exploratory Data Analysis —Modelling

> | Python · Pandas · Seaborn · Matplotlib · Sckitlearn|

> This project performs a comprehensive Exploratory Data Analysis (EDA) on the Healthcare Stroke Dataset to identify key risk factors associated with stroke incidence. The analysis covers data cleaning, feature engineering, statistical exploration, and multivariate visual analysis.
The project predicts the likelihood of stroke using patient demographic and health indicators. The goal is to develop a model capable of early stroke risk identification, supporting preventive healthcare decision-making.

---

## Project Overview
Stroke remains one of the leading causes of death and long-term disability worldwide. Early identification of individuals at high risk can significantly improve preventive interventions.

This project applies machine learning techniques to analyze patient health data and build predictive models capable of identifying stroke risk with high reliability.

The workflow includes:

Data exploration and preprocessing

Feature engineering

Handling class imbalance

Model training and evaluation

Performance comparison across algorithms

---
## Objectives

- Identify key health and demographic factors associated with stroke risk.

- Build robust predictive models for stroke classification.

- Address class imbalance, which is common in medical datasets.

- Evaluate models using reliable metrics beyond accuracy.

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
## Project Pipeline

### Data Cleaning & Preprocessing

#### Handling missing BMI values
- Only `bmi` had missing values: **201 rows (~3.93%)**
- **Strategy:** Median imputation grouped by engineered age group to account for outliers and the age–BMI relationship

### Feature transformation
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

###  Cleaning
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

## Residence Type Insights

The dataset includes a **Residence_type** feature representing whether a patient lives in an **Urban** or **Rural** environment.

Key observations from the analysis:

- Urban residents show **slightly higher stroke incidence** compared to rural populations.
- Urban hypertensive patients demonstrate a **higher stroke prevalence** than rural hypertensive patients.
- Differences may reflect **lifestyle patterns, healthcare access, stress levels, and environmental factors**.

This feature was explored through categorical comparisons and cross-tabulation with hypertension, age groups, and stroke outcomes.

---

## Visualization

Alongside traditional visualization libraries, **Plotly Express** was used to create **interactive visualizations** for deeper exploration of stroke risk factors.

Interactive plots help analyze relationships between variables such as:

- Age distribution vs stroke occurrence
- Glucose levels across stroke categories
- Urban vs Rural stroke incidence
- Smoking status vs stroke rate

Plotly Express enables dynamic exploration of healthcare data patterns through interactive charts.

---

# Modeling Approach
## Data Preparation
### Dataset Encoding
- LabelEncoder is used to convert categorical text data into numeric values. Each unique category is mapped to an integer from 0 to n-classes.
### Dataset normalization
 - StandardScaler to normalize numerical variable

### Handling Class Imbalance
Medical datasets often contain far fewer positive cases.

To address this:

- SMOTE (Synthetic Minority Oversampling Technique) was used

- This technique generates synthetic examples of the minority class to improve model learning.

- Train-test split

### Machine Learning Models
Multiple algorithms were trained and evaluated:

| Model                                   | Description                                                                                 |
| --------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Logistic Regression (LR)**            | Baseline statistical classifier used for interpretable linear relationships.                |
| **Decision Tree (DT)**                  | Tree-based model that captures nonlinear feature interactions.                              |
| **Random Forest (RF)**                  | Ensemble of decision trees that reduces overfitting and improves predictive performance.    |
| **Gradient Boosting (GB)**              | Sequential ensemble model that improves prediction by correcting previous errors.           |
| **K-Nearest Neighbors (KNN)**           | Distance-based algorithm that classifies samples based on similarity to neighboring points. |
| **Extreme Gradient Boosting (XGBoost)** | Optimized gradient boosting framework known for high predictive power.                      |

 

### Model Evaluation Metrics

Because the dataset is imbalanced, accuracy alone is misleading. Therefore the models were evaluated using:

Precision

Recall

F1 Score

ROC–AUC

Confusion Matrix

Given the strong class imbalance, **recall and F1 score are more meaningful metrics than accuracy alone.** These metrics provide a better understanding of how well the model detects stroke cases.

---

## Best Performing Model
Random Forest Classifier

Performance metrics on the test set:
| Metric    | Score    |
| --------- | -------- |
| Accuracy  | ~94%   |
| Precision | 93%   |
| Recall    | 95%   |
| F1 Score  | 0.94    |
| ROC-AUC   | **0.98** |

The model correctly classified 904 non-stroke cases and 919 stroke cases. It produced only 68 false positives and missed 53 stroke cases.The model demonstrates excellent discrimination ability, with an AUC of 0.98, meaning it can almost perfectly distinguish stroke from non-stroke patients.

## Ethical Considerations
While predictive models can support healthcare decisions, several ethical considerations remain important:

- Model bias may arise from demographic imbalances in training data.

- Predictions should support clinicians, not replace them.

- False negatives may delay treatment, while false positives may cause unnecessary concern.

Therefore, such models should be deployed alongside clinical expertise and proper validation.



## Technology Stack

| Library | Purpose |
|---|---|
| `pandas` | Data loading, manipulation, and groupby operations |
| `numpy` | Numerical computations |
| `matplotlib` | Line plots, bar charts, and custom visualisations |
| `seaborn` | Statistical plots (boxplot, histplot, heatmap, countplot) |
| `plotly.express` | Interactive visualizations for exploratory analysis |
| `scikit-learn` | Machine learning modelling and evaluation |
| `Imbalanced-learn` | For imbalanced target vector|
| `XGBoost` | Machine learning modelling and evaluation |

---

```markdown
Stroke-Risk-Prediction/
│
├── healthcare-dataset-stroke-data.csv/
├── Best_Brains-strokerisk.ipynb/
├── plots.zip/
├── README.md
└── requirements.txt

````

---

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/stroke-prediction-analysis.git
````

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn plotly scikit-learn
```

### 3. Run the notebook

```bash
jupyter notebook
```

Open `stroke_risk.ipynb` to explore the full analysis.

---

## Future Improvement
- External dataset validation
-  Real-time deployment via clinical dashboards

## Conclusions
Stroke incidence in this dataset is primarily driven by **clinical and age-related factors** rather than lifestyle variables alone:

* Stroke risk is predominantly **age-driven** with a non-linear acceleration after **55–60**
* **Cardiovascular disease** (heart disease + hypertension) significantly amplifies risk
* **Elevated glucose levels** represent a strong biochemical marker for stroke
* Lifestyle factors (BMI, smoking) contribute **secondary risk**, often interacting with age
* **Class imbalance (~95:5)** must be addressed in any downstream predictive modelling
This project demonstrates the potential of machine learning in early stroke risk prediction. By combining proper data preprocessing, imbalance handling, and ensemble learning techniques, the model achieves high predictive accuracy and excellent AUC performance.

Such predictive tools can play a valuable role in preventive healthcare and risk screening systems.
---



