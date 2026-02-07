# Health Analytics and Classification using NHANES 2017–2020 Data

An end-to-end health analytics project that merges multiple NHANES (National Health and Nutrition Examination Survey) datasets, engineers a composite health score, and predicts health categories using machine learning. The project includes data wrangling, visualization (matplotlib, Bokeh, HoloViews), ordinal logistic regression, random forest classification, SMOTE for class balancing, and interactive dashboards.

---

## Table of Contents

- [Overview](#overview)
- [What I Did](#what-i-did)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Installation & Setup](#installation--setup)
- [How to Run](#how-to-run)
- [Key Results](#key-results)
- [Visualizations](#visualizations)
- [References & Documentation](#references--documentation)
- [License](#license)

---

## Overview

This project uses **NHANES 2017–March 2020 Pre-Pandemic** data to:

1. **Merge** five health-related datasets (body measures, mental health, alcohol use, sleep, cholesterol).
2. **Engineer** a composite `Health_Score` from BMI, mental health, alcohol intake, sleep duration, and total cholesterol.
3. **Classify** individuals into three health categories: **Healthy**, **Unhealthy**, and **Severely Unhealthy**.
4. **Model** health category using **Ordinal Logistic Regression** and **Random Forest**, with **SMOTE** for class balancing.
5. **Visualize** results with static (matplotlib) and interactive (Bokeh, HoloViews, Panel) plots, including ROC curves and an interactive health estimator.

---

## What I Did

### Data Processing
- Loaded five NHANES SAS XPT files: Body Measures (P_BMX), Mental Health (P_DPQ), Alcohol (P_ALQ), Sleep (P_SLQ), Cholesterol (P_TCHOL).
- Cleaned and standardized variables (binary encoding for mental health, KNN imputation for alcohol, renaming and unit handling for cholesterol and sleep).
- Merged all datasets on participant ID (`SEQN`) and dropped missing values to obtain a unified analytic dataset (~8,095 complete records).

### Health Scoring
- Defined category scores (1–3) for:
  - **BMI**: Underweight/Normal/Overweight/Obese  
  - **Mental health**: No concern vs. concern  
  - **Alcohol**: Rare to frequent  
  - **Sleep**: Ideal / slight deviation / poor  
  - **Cholesterol**: Desirable / borderline / high  
- Computed overall `Health_Score` as the mean of these five scores.
- Mapped `Health_Score` to labels: **Healthy** (≤1.66), **Unhealthy** (1.67–2.33), **Severely Unhealthy** (>2.33).

### Machine Learning
- **Class imbalance**: Applied **SMOTE** on the training set to balance the three health categories.
- **Ordinal Logistic Regression** (statsmodels `OrderedModel`): Used to model ordered health categories and assess feature importance (e.g. mental health, cholesterol).
- **Outlier removal**: Flagged test samples with large prediction errors (z-score > 1.5) and refined the dataset for the final model.
- **Random Forest**: Trained with 100 trees on selected features; evaluated with accuracy, MSE, log loss, and Nagelkerke R²; reported feature importances and cross-validation scores.
- **ROC/AUC**: Computed ROC curves and AUC for each health class (one-vs-rest); achieved very high AUC (>0.99) for all classes.

### Visualizations
- **Health category distribution**: Bar chart (matplotlib + HoloViews) of Healthy / Unhealthy / Severely Unhealthy counts.
- **Feature importance**: Horizontal bar chart of Random Forest feature importances.
- **ROC curves**: Static matplotlib plot and **HoloViews** version saved to `roc_curves_holoviews.html` for interactive viewing in a browser.
- **Health score heatmap**: Bokeh heatmap of participants × health category with color by score.
- **Interactive health estimator**: Bokeh sliders for BMI, mental health, alcohol, sleep, and cholesterol with real-time risk classification (Healthy / At Risk / Critically Unhealthy).

### Ethics & Privacy
- The notebook includes a section on **data privacy and ethical concerns** (informed consent, anonymization, algorithmic bias, security, misuse, transparency, and impact on vulnerable populations) with brief mitigation strategies.

---

## Dataset

All data are from **NHANES 2017–March 2020 Pre-Pandemic** (CDC NCHS). Files are in SAS XPT format.

| File       | Description                     | Key Variables                          |
|-----------|----------------------------------|----------------------------------------|
| `P_BMX.xpt`  | Body measures                   | SEQN, BMXBMI, BMXHT, BMXWT             |
| `P_DPQ.xpt`  | Mental health (depression)      | SEQN, DPQ010–DPQ100                    |
| `P_ALQ.xpt`  | Alcohol use                     | SEQN, ALQ121, etc.                     |
| `P_SLQ.xpt`  | Sleep disorders                 | SEQN, SLD012 (weekday sleep duration)  |
| `P_TCHOL.xpt`| Total cholesterol               | SEQN, LBDTCSI (mmol/L)                 |

- **Source**: [CDC NHANES](https://wwwn.cdc.gov/nchs/nhanes/)  
- **Citation**: National Center for Health Statistics. National Health and Nutrition Examination Survey. Hyattsville, MD: CDC.

---

## Project Structure

```
.
├── README.md                    # This file
├── requirements.txt             # Python dependencies
├── Health Analytics and Classification using NHANES 2017–2020 Data.ipynb   # Main notebook
├── P_BMX.xpt                    # Body measures
├── P_DPQ.xpt                    # Mental health
├── P_ALQ.xpt                    # Alcohol use
├── P_SLQ.xpt                    # Sleep
├── P_TCHOL.xpt                  # Total cholesterol
├── 2c541dd0-9292-49a5-a3a2-52b360274f71.pdf
├── 4bc75bc9-e4d2-4db3-98db-7d29ed0c5044.pdf
└── roc_curves_holoviews.html    # Generated: interactive ROC plot (after running notebook)
```

---

## Requirements

- **Python**: 3.8+
- **Libraries**: See `requirements.txt`

| Package        | Purpose                    |
|----------------|----------------------------|
| pandas         | Data loading & manipulation|
| numpy          | Numerical operations       |
| pyreadstat     | Reading SAS XPT files      |
| scikit-learn   | ML, metrics, preprocessing |
| imbalanced-learn | SMOTE, pipelines        |
| statsmodels    | Ordinal logistic regression|
| bokeh          | Interactive plots         |
| holoviews      | Declarative viz (Bokeh)    |
| panel          | Dashboards & widgets       |
| matplotlib     | Static plots               |
| jupyter        | Notebook execution         |
| nbconvert      | Optional: run notebook from CLI |

---

## Installation & Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/health-analytics-nhanes-2017-2020.git
   cd health-analytics-nhanes-2017-2020
   ```

2. **Create a virtual environment (recommended)**
   ```bash
   python -m venv venv
   venv\Scripts\activate          # Windows
   # source venv/bin/activate     # macOS/Linux
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. Ensure the five `.xpt` files and the notebook are in the same directory (or adjust paths in the notebook).

---

## How to Run

### Option 1: Jupyter / Cursor / VS Code
1. Open `Health Analytics and Classification using NHANES 2017–2020 Data.ipynb`.
2. Run all cells (e.g. **Run All**).
3. Static plots (matplotlib) will appear in the notebook. Bokeh/HoloViews widgets may require a browser; the notebook also saves the HoloViews ROC plot to `roc_curves_holoviews.html` and can open it in your browser.

### Option 2: Command line (execute without opening the notebook)
```bash
python -m nbconvert --to notebook --execute --inplace "Health Analytics and Classification using NHANES 2017–2020 Data.ipynb"
```
Note: Some interactive widgets will not render in headless execution; use the notebook UI for full interactivity.

### Viewing the HoloViews ROC curve
- After running the notebook, open `roc_curves_holoviews.html` in a web browser for the interactive Bokeh/HoloViews ROC plot.

---

## Key Results

- **Merged dataset**: ~8,095 participants with complete data across all five domains.
- **Health distribution**: Unhealthy (largest), Healthy, then Severely Unhealthy.
- **Ordinal model**: Mental health, alcohol, and cholesterol were strong predictors; BMI had smaller direct effect in the ordinal model.
- **Random Forest**: Very high accuracy and AUC; feature importance highlighted **Weekday Sleep Duration**, **BMI**, **Mental Health Score**, and **Total Cholesterol**.
- **SMOTE**: Successfully balanced training set for ordinal and Random Forest models.

---

## Visualizations

- **Health category bar chart**: Counts for Healthy / Unhealthy / Severely Unhealthy (matplotlib + HoloViews).
- **Feature importance bar chart**: Random Forest importances (matplotlib + HoloViews).
- **ROC curves**: One curve per health class + diagonal; static in notebook, interactive in `roc_curves_holoviews.html`.
- **Health score heatmap**: Participants × category, color by score (Bokeh).
- **Interactive health estimator**: Sliders for BMI, mental health, alcohol, sleep, cholesterol → Healthy / At Risk / Critically Unhealthy (Bokeh).

---

## References & Documentation

- [NHANES 2017–March 2020 Pre-Pandemic](https://wwwn.cdc.gov/nchs/nhanes/Default.aspx)
- [NHANES Questionnaires, Datasets, and Documentation](https://wwwn.cdc.gov/nchs/nhanes/Default.aspx)
- [P_BMX](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Examination&Cycle=2017-2020), [P_DPQ](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Questionnaire&Cycle=2017-2020), [P_ALQ](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Questionnaire&Cycle=2017-2020), [P_SLQ](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Questionnaire&Cycle=2017-2020), [P_TCHOL](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/P_TCHOL.htm)

The included PDFs in the repository are project documentation or reports (see filenames for context).

---

## License

This project is for educational and research purposes. NHANES data are in the public domain; see [CDC/NCHS data use policies](https://www.cdc.gov/nchs/nhanes/). Code in this repository is provided as-is.
