---
CVE Severity Prediction Workflow ---
Please uncomment the function(s) you want to run below.
Downloading sample Garak report...
✅ Downloaded: data\garak\garak.report.jsonl
✅ Garak report successfully converted to: data\garak\garak_report_flat.csv
---
Downloading: [https://nvd.nist.gov/feeds/json/cve/1.1/nvdcve-1.1-2025.json.gz](https://nvd.nist.gov/feeds/json/cve/1.1/nvdcve-1.1-2025.json.gz)
 -> Successfully saved to data\nvd_data\nvdcve-1.1-2025.json.gz
--- Download Process Complete ---

--- Starting NVD Data Parsing ---
Parsing: nvdcve-1.1-2025.json.gz

--- Removing Duplicates ---
Number of entries before duplicate removal: 5465
Number of entries after duplicate removal: 4968

--- Parsing Complete. Saved 4968 unique entries to data\nvd_data\all_nvd_cves.pkl ---
Loading parsed data from data\nvd_data\all_nvd_cves.pkl...

Training on 4968 valid NVD entries after cleaning.
Loading embedding model: 'all-MiniLM-L6-v2'...

!!! WARNING: Encoding all descriptions will take a very long time and consume significant memory. Please be patient. !!!

Batches: 100%|██████████| 156/156 [04:03<00:00,  1.56s/it]

Applying SMOTE to balance the training data...
SMOTE balancing complete. Training set size is now: (6212, 384)

--- Training Logistic Regression ---
              precision    recall  f1-score   support

    Critical       0.60      0.70      0.65       268
        High       0.58      0.53      0.55       406
         Low       0.19      0.59      0.29        51
      Medium       0.73      0.56      0.63       517

    accuracy                           0.58      1242
   macro avg       0.52      0.60      0.53      1242
weighted avg       0.63      0.58      0.60      1242

--- Training Random Forest ---
              precision    recall  f1-score   support

    Critical       0.74      0.63      0.68       268
        High       0.59      0.56      0.58       406
         Low       0.41      0.27      0.33        51
      Medium       0.68      0.77      0.72       517

    accuracy                           0.65      1242
   macro avg       0.60      0.56      0.58      1242
weighted avg       0.65      0.65      0.65      1242

--- Training LightGBM (Tuned) ---
[LightGBM] [Info] Auto-choosing col-wise multi-threading, the overhead of testing was 0.037424 seconds.
You can set `force_col_wise=true` to remove the overhead.
[LightGBM] [Info] Total Bins 97920
[LightGBM] [Info] Number of data points in the train set: 6212, number of used features: 384
[LightGBM] [Info] Start training from score -1.386294
[LightGBM] [Info] Start training from score -1.386294
[LightGBM] [Info] Start training from score -1.386294
[LightGBM] [Info] Start training from score -1.386294

[c:\Users\DELL\anaconda3\envs\nlp\lib\site-packages\sklearn\utils\validation.py:2739](file:///C:/Users/DELL/anaconda3/envs/nlp/lib/site-packages/sklearn/utils/validation.py:2739): UserWarning: X does not have valid feature names, but LGBMClassifier was fitted with feature names
  warnings.warn(

    precision    recall  f1-score   support

    Critical       0.70      0.62      0.66       268
        High       0.59      0.58      0.58       406
         Low       0.38      0.24      0.29        51
      Medium       0.69      0.77      0.73       517

    accuracy                           0.65      1242
   macro avg       0.59      0.55      0.56      1242
weighted avg       0.65      0.65      0.65      1242

🏆 Best performing model is: Random Forest

Retraining Random Forest on the full resampled dataset for final model...
✅ Best model saved to models\best_cvss_classifier_historic.pkl
Loading saved model from models\best_cvss_classifier_historic.pkl...
Loading embedding model for prediction...
