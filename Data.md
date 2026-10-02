# Data Overview

This project uses phishing-URL datasets from two sources:

- UCI Phishing Websites dataset: the main training dataset loaded from `Training Dataset.arff` and processed into `processed_phishing_data.csv`
- PhiUSIIL phishing URL dataset: external dataset stored as `PhiUSIIL_Phishing_URL_Dataset.csv`

The notebook workflow combines both datasets for model training, duplicate-cleaning, and transfer-learning experiments.

---

## 1. Datasets in the workspace

### UCI phishing dataset
- File: `Training Dataset.arff`
- Processed output: `processed_phishing_data.csv`
- Label column: `Result`
- Feature count: 30 URL/website attributes
- Target type: binary classification

The raw ARFF file was converted into a pandas DataFrame, byte-string values were decoded, and feature columns were normalized to numeric values before modeling.

### PhiUSIIL dataset
- File: `PhiUSIIL_Phishing_URL_Dataset.csv`
- Used for cross-dataset validation and transfer experiments
- Contains URL-level indicators such as `IsDomainIP`, `IsHTTPS`, `NoOfSubDomain`, `URLLength`, `HasFavicon`, `NoOfiFrame`, and `NoOfURLRedirect`
- Label is converted to a phishing indicator in the notebook so it is compatible with the UCI-style binary target convention

---

## 2. Data preparation steps

The notebook applies these preprocessing rules:

1. Load the ARFF file with `scipy.io.arff`
2. Convert byte-string values into regular strings
3. Convert all feature columns to numeric values
4. Split into feature matrix `X` and target vector `y`
5. Perform train/test splitting using `train_test_split` with `stratify=y` and `random_state=42`
6. Train multiple classifiers on the cleaned data

---

## 3. Target encoding and label handling

The target column is treated as binary:

- 1 = phishing / malicious
- 0 = legitimate / benign

In the notebook, some steps normalize labels by decoding bytes and converting them to numeric values. Duplicate-removal and transfer experiments also use consistent binary label mapping so that the classifier outputs can be compared across datasets.

---

## 4. Duplicate-removal analysis

The project performs systematic duplicate checks:

- exact row duplicates are removed
- duplicated feature patterns are examined
- conflicting patterns (same feature vector with different labels) are identified and removed in later experiments

This produces a deduplicated dataset used for the final model comparisons. The notebook records:

- original row count
- duplicate row count
- rows removed after deduplication
- percentage removed

This step helps prevent leakage and improves the reliability of evaluation metrics.

---

## 5. Feature mapping between UCI and PhiUSIIL

A cross-dataset transfer study maps a subset of common indicators between the two sources.

Example common features used in the notebook:

- `has_ip`
- `is_https`
- `has_subdomain`
- `has_favicon`
- `has_popup`
- `has_iframe`
- `has_redirect`
- `url_over_75`

The notebook explicitly notes that several mappings are only approximate or proxy-based, not exact semantic equivalents. For example:

- `HTTPS_token` is not the same as `IsHTTPS`
- `Favicon` and `HasFavicon` are not perfectly equivalent
- `popUpWidnow` and `NoOfPopup` differ in meaning
- URL length thresholds were considered with special care because the datasets use different length conventions

This means the transfer-learning results are best interpreted as comparative, not perfectly one-to-one mappings.

---

## 6. Experimental dataset variants

The notebook evaluates several data variants:

- original UCI data
- deduplicated UCI data
- deduplicated UCI data with conflicting patterns removed
- three-feature aligned representation for cross-dataset testing
- both directions of transfer:
  - UCI → PhiUSIIL
  - PhiUSIIL → UCI

The final analyses track metrics such as:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC

---

## 7. Main files produced

The workspace includes several output artifacts created during the experiments:

- `baseline_model_results.csv`
- `processed_phishing_data.csv`
- `stage3_cross_validation_results.csv`
- `before_after_duplicate_removal.csv`
- `phius_common_8_features.csv`

These files store model performance and processed feature representations used in the analysis.

---

## 8. Notes for interpretation

The dataset is a research-oriented benchmark designed for phishing detection experiments. The project emphasizes:

- model comparison across multiple classifiers
- duplicate and conflict removal effects
- cross-dataset generalization under intentionally imperfect feature alignment
- interpretability via feature importance and evasion analysis

The notebook notes that some mappings across datasets are approximate, so direct model transfer should be interpreted cautiously unless the feature semantics are verified against the original data definitions.
