# phishing-benchmark-audit
Code for "Robust Phishing Website Detection: Benchmark Integrity,
Feature-Schema Compatibility, and Feature-Space Perturbation Analysis".

## Run
- Python 3.13.14, scikit-learn 1.8.0, XGBoost 3.4.1 (see requirements.txt)
- Download data (not included):
  - UCI Phishing Websites (ID 327): Training Dataset.arff
  - UCI PhiUSIIL (ID 967): PhiUSIIL_Phishing_URL_Dataset.csv
- Place both files next to Training.ipynb, then Run All.
- Seed 42. XGBoost results may differ in the 3rd–4th decimal between runs.

## Data and license
Datasets are CC BY 4.0 (see DATA.md). Code: MIT
