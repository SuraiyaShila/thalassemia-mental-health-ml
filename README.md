# A Trustworthy Machine Learning Framework for Mental Health Classification of Thalassemia Patients in Bangladesh

Code for our conference paper. The framework classifies the mental health status (Good vs. Bad/at-risk) of thalassemia patients from structured clinical and socio-demographic records, and audits the model for **privacy, explainability, calibration, uncertainty and fairness**, not just accuracy.

## Why "trustworthy"?
A high accuracy score alone is not enough in sensitive medical care. Patient data can allow re-identification, hidden decision logic reduces clinician trust, poorly calibrated predictions give false confidence, and models that work better for some groups can worsen health inequalities. This project examines these dimensions together.

## Pipeline
1. **Privacy-aware ingestion:** Patient_ID is pseudonymized with a salted SHA-256 hash and excluded from modeling features. Missing values are encoded with clinically informed defaults.
2. **Clinician-guided feature engineering:** disease duration, ferritin-to-hemoglobin ratio, transfusion burden, ordinal and one-hot encoding.
3. **Modeling:** Random Forest, Extra Trees, XGBoost, CatBoost, SVM and MLP with a stratified 80/20 split and 5-fold cross-validation. Class imbalance is handled by class-weighting, not SMOTE.
4. **Leakage control:** Thalassemia_Type and Splenectomy_Status are almost deterministically tied to the label, so a leakage-controlled setting without them is also reported.
5. **Trustworthiness audit:**
   - **Explainability:** SHAP (global) and LIME (patient-level)
   - **Calibration and uncertainty:** Brier score and tree-level prediction std. dev.
   - **Fairness:** demographic parity and equalized odds differences across gender and residence
   - **Privacy:** k-anonymity analysis of quasi-identifiers (age, gender, residence, education)

## Key results (617 patients, 124 test)
- Random Forest: 97.6% accuracy, 0.997 AUC-ROC on the held-out test set
- Leakage-controlled: tree ensembles and SVM keep cross-validated ROC-AUC of at least 0.998
- Brier score: 0.0178
- Fairness: modest gender gap, larger residence gap (DPD=0.350, EOD=0.182) that needs investigation
- Privacy: minimum k = 1, so further generalization is needed for formal privacy guarantees

## Data and confidentiality
- The dataset is the public *Bangladesh Thalassemia Dataset* (Mendeley Data): https://doi.org/10.17632/8kcdkxmcjw.1
- **No raw patient data is stored in this repository.** Download the dataset from the link above and place the CSV in the working directory.
- Patient identifiers are pseudonymized and the hashing salt is never committed.
- Ethical clearance and informed consent were obtained from all participants (see paper).

## How to run
1. Open `notebooks/Thalassemia.ipynb` in Google Colab.
2. Install dependencies: `pip install -r requirements.txt`
3. Upload `Thalassemia_Bangladesh_dataset.csv` when prompted and run all cells.

All models use `random_state=42` for reproducibility.

## Limitations
Single-center dataset, so external multi-center validation is still needed. The framework is a decision-support aid, not a replacement for clinical judgment.

## Citation
If you use this code, please cite our paper (details will be added after publication).
