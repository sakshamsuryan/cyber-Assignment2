# Cyber 2: Hyperparameter Optimization for PhishGuard

This project focuses on improving the machine learning model used in the PhishGuard phishing URL detection system (Assignment 1). It applies hyperparameter optimization techniques and evaluates the performance of the improved model against the original baseline model.

## Project Overview

The project performs the following operations:

1. Extracts the same 23 URL-based features used by the application through `features.py`.
2. Develops a baseline model using `HistGradientBoostingClassifier` with default configurations and one-hot encoding for TLD values.
3. Implements `RandomizedSearchCV` to explore 30 randomly selected hyperparameter combinations using 3-fold cross-validation on the training dataset. The optimization process uses phishing-class F1-score as its evaluation metric.
4. Tests both the baseline and optimized models using an identical, previously untouched test dataset.
5. Exports the optimized model, performance comparison results, and graphical analysis.

## Project Files

| File Name | Description |
|---|---|
| `cyber2_tuning.py` | Primary Python script for executing the complete tuning process. It can also be executed section by section in VS Code or Spyder using `# %%` cells. |
| `cyber2_hyperparameter_tuning.ipynb` | Notebook version of the project, compatible with Jupyter Notebook and Google Colab. |
| `features.py` | Contains the functions responsible for extracting URL-related features. |
| `PhiUSIIL_Phishing_URL_Dataset.csv` | Required dataset file, which must be downloaded and added manually. |

## Installation and Configuration

Install the required Python libraries using the following command:

```bash
pip install scikit-learn==1.9.1 pandas numpy scipy matplotlib joblib
```

**Important:** Use `scikit-learn==1.9.1` to maintain compatibility with the application's existing requirements. Using another version may cause problems when loading the saved model.

Before execution, ensure that `features.py` and `PhiUSIIL_Phishing_URL_Dataset.csv` are available in the same directory as the main Python script.

## Executing the Project

Run the following command from the terminal:

```bash
python cyber2_tuning.py
```

Expected execution time:

- Feature extraction and dataset preparation generally require around one minute.
- Hyperparameter optimization may take approximately 10–20 minutes, depending on system performance.
- If the system has limited processing power, reduce `N_ITER` from 30 to 10 to shorten execution time.

## Generated Results

After successful execution, the project generates the following output files:

| Output File | Description |
|---|---|
| `url_model_tuned.pkl` | Contains the optimized machine learning pipeline, which can replace the existing `url_model.pkl`. |
| `cyber2_results.csv` | Stores the evaluation metrics of both baseline and optimized models. |
| `cyber2_comparison.png` | Displays visual comparisons of model metrics, classification errors, and ROC curves. |

## Hyperparameters Selected for Optimization

The following parameters are explored during the randomized search:

| Hyperparameter | Values / Search Interval |
|---|---|
| `learning_rate` | 0.02–0.3 (logarithmic scale) |
| `max_iter` | 200, 400, 600, 800 |
| `max_leaf_nodes` | 15, 31, 63, 127 |
| `max_depth` | None, 6, 10, 15 |
| `min_samples_leaf` | 10, 20, 50, 100 |
| `l2_regularization` | 0.001–10 (logarithmic scale) |
| TLD `min_frequency` | 5, 20, 50, 200 |

## Model Evaluation Criteria

Phishing detection is considered the primary classification objective. The dataset uses the following label mapping:

- `label = 0`: Phishing URL (positive class).
- `label = 1`: Legitimate URL.

This encoding is consistent with the implementation used in `app.py`.

The following performance indicators are calculated:

- **Accuracy:** Measures the overall percentage of correctly classified URLs.
- **Precision:** Indicates how many URLs predicted as phishing are actually phishing.
- **Recall:** Measures the proportion of phishing URLs successfully detected.
- **F1-Score:** Combines precision and recall into a single performance measure.
- **ROC-AUC:** Evaluates the model's ability to distinguish between phishing and legitimate URLs.
- **PR-AUC:** Measures performance by considering the relationship between precision and recall.
- **False Negatives (FN):** Represents phishing URLs incorrectly classified as safe websites.
- **False Positives (FP):** Represents legitimate websites incorrectly flagged as phishing.
- **Training F1 vs Testing F1:** Helps identify potential overfitting by comparing training and testing performance.
- **Training Time:** Records the time required to fit the model.

## Performance Comparison with Assignment 1

To maintain a reliable comparison, the baseline model is trained again using the same dataset split as the optimized model.

If the performance results from Assignment 1 were obtained using a different train-test split, those earlier values can be added to the `PREV` variable in the script. This allows the previous results to be displayed alongside the newly generated model comparisons.

## Integrating the Optimized Model into PhishGuard

Once the tuning process is complete, the improved model can be connected to the existing application.

Follow these steps:

1. Move `url_model_tuned.pkl` into the PhishGuard application directory.
2. Rename the file to `url_model.pkl`, or modify the model filename inside the `load_model()` function in `app.py`.
3. Run the application using the updated model.

No modifications to the existing feature extraction process or label encoding are necessary because the optimized pipeline follows the same input structure as the original model.

## Limitations and Important Observations

- The PhiUSIIL dataset contains highly informative URL-related features, so hyperparameter optimization may produce only a small improvement over the default model.
- Some URLs in the dataset may be repeated. Such duplicates can influence evaluation results and potentially increase the reported test performance.
- Removing duplicate URLs before dividing the dataset into training and testing sets could help produce a more reliable evaluation.
- The machine learning model primarily analyzes URL-based information and cannot independently verify every aspect of a website.
- Additional security checks performed by the PhishGuard application, including DNS, WHOIS, SSL, and webpage content analysis, provide further protection beyond URL-based prediction.

**Overall Objective:** The main goal of Cyber 2 is to improve the reliability of the PhishGuard phishing detection system through systematic hyperparameter optimization and a performance-based comparison with the original machine learning model.
