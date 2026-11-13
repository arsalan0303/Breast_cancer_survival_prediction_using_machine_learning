# Breast Cancer Survival Status Classification

## Project overview

This project develops and evaluates machine-learning classification models using a breast cancer clinical dataset. The prediction target is `Overall Survival Status`, with the recorded classes `Living` and `Deceased`.

The notebook follows an end-to-end workflow covering data loading, data-quality checks, exploratory data analysis (EDA), leakage-aware feature selection, feature engineering, preprocessing, model training, evaluation, and model comparison.

**Project scope:** This notebook performs binary classification of the recorded survival-status label. It does not perform time-to-event survival analysis, estimate survival duration, or account for right-censoring.

> **Disclaimer:** This is an educational and analytical project, not a clinically validated decision-support system. Its predictions must not be used to guide patient care.

## Notebook workflow

1. **Import libraries** — load Python libraries for data manipulation, visualization, and machine learning.
2. **Load data** — read the breast cancer CSV file and inspect its initial records and dimensions.
3. **Data quality checks** — review data types, missing values, and duplicate records.
4. **Data cleaning** — clean categorical values and convert common missing-value markers to null values.
5. **Target analysis** — inspect the distribution of `Overall Survival Status` and remove records without a target label.
6. **Leakage-aware feature selection** — exclude patient identifiers, direct/future outcome information, recurrence follow-up fields, post-diagnosis treatment variables, and cohort metadata.
7. **Exploratory data analysis** — visualize missingness, numerical distributions, outliers, age versus tumour size, and numerical correlations.
8. **Feature engineering** — create log-transformed tumour-size and positive lymph-node-count features, and an exploratory age-group feature. Remove all-missing and constant predictors.
9. **Train/test split** — create a stratified 80/20 training and holdout-test split.
10. **Preprocessing** — median-impute and standardize numerical features; impute missing categorical values and apply one-hot encoding. Preprocessing is included inside scikit-learn pipelines to avoid fitting transformations on the test set.
11. **Model development** — train:
    - Logistic Regression with class weighting.
    - Random Forest Classifier with class weighting.
12. **Model evaluation** — use stratified five-fold cross-validation on the training data and evaluate holdout performance using accuracy, balanced accuracy, ROC-AUC, F1-score, classification reports, confusion matrices, and ROC curves.
13. **Model comparison and example prediction** — compare holdout metrics, save the comparison table, select a demonstration model by holdout ROC-AUC, and show an example prediction for a held-out observation.

## Leakage prevention and prediction scope

The notebook frames the task as an exploratory diagnosis-time classification exercise. It excludes the following fields when present:

- Patient identifiers (`Patient ID`, `Patient_ID`, `Patient ID Number`, `ID`, `id`)
- `Patient's Vital Status`
- `Overall Survival (Months)`
- `Relapse Free Status`
- `Relapse Free Status (Months)`
- Post-diagnosis treatment fields: `Type of Breast Surgery`, `Chemotherapy`, `Hormone Therapy`, and `Radio Therapy`
- `Cohort` metadata

These exclusions are intended to prevent the model from using direct target proxies, future follow-up information, or variables that may not be available at diagnosis.

## Feature engineering

The notebook preserves the original measurements and adds:

- `Log Tumor Size`: `log1p` transformation of tumour size.
- `Log Positive Lymph Nodes`: `log1p` transformation of the number of positive lymph nodes.
- `Age Group`: exploratory categories based on age at diagnosis.

The age bands are modelling features for exploration, not validated clinical thresholds.

## Models and evaluation

| Area | Method |
|---|---|
| Classification models | Logistic Regression, Random Forest |
| Validation | Stratified 5-fold cross-validation on training data |
| Holdout split | 80% train / 20% test, stratified |
| Metrics | Accuracy, balanced accuracy, ROC-AUC, macro F1 |
| Diagnostics | Classification report, confusion matrix, ROC curve |
| Output | `model_comparison.csv` |

The positive class for ROC-AUC is explicitly set to `Living` in the notebook. Review the class definition and the clinical interpretation of the target before drawing conclusions from metrics.

## Project files

Keep the notebook and CSV dataset in the same folder:

```text
Breast-Cancer-Classification/
├── Breast_Cancer_Project_Fixed(2).ipynb
├── Breast Cancer(1).csv
├── README.md
├── requirements.txt
└── model_comparison.csv   # generated when the notebook runs
```

The notebook searches for `Breast Cancer.csv` or `Breast Cancer(1).csv`. If your file has a different name, update the `candidate_paths` list in the data-loading cell.

## Requirements

- Python 3.9 or newer recommended.
- Jupyter Notebook or Visual Studio Code with the Jupyter extension.

Install the dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## Run the notebook

1. Place the CSV dataset in the same directory as the notebook.
2. Create and activate a Python virtual environment (recommended).
3. Install dependencies from `requirements.txt`.
4. Open `Breast_Cancer_Project_Fixed(2).ipynb` in Jupyter or VS Code.
5. Select the environment containing the installed packages as the notebook kernel.
6. Run all cells from top to bottom.

The notebook writes `model_comparison.csv` to its current working directory.

## Limitations and responsible interpretation

- The target is a recorded binary status label; it is not a defined five-year survival endpoint.
- Binary classification does not explicitly model follow-up time or right-censoring.
- A random train/test split and internal cross-validation do not constitute external validation.
- The age groups are exploratory and should not be interpreted as clinical cutoffs.
- Predictive performance depends on dataset quality, class balance, follow-up patterns, cohort selection, and feature availability.
- A high score does not establish clinical usefulness or causality.
- Confirm the dataset's licence, provenance, and permitted use before sharing it. Do not publish patient-level or sensitive data without appropriate authorization.

For a time-to-event research question, use a dedicated survival-analysis workflow with a time variable, event indicator, censoring-aware methods (for example, Kaplan–Meier and Cox proportional hazards), assumption checks, and suitable validation.

## Main libraries

- pandas
- NumPy
- Matplotlib
- Seaborn
- scikit-learn
- Jupyter
