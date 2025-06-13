# 📘 JPM AI Vulnarability Scoring: Model Comparison & Final Recommendation

## 📊 Objective

This project aims to predict the **CVSS severity** levels (`Low`, `Medium`, `High`, `Critical`) for CVEs (Common Vulnerabilities and Exposures) using different deep embedding models and machine learning classifiers trained on historic NVD data, benchmarked against failed test cases from a Garak LLM red-teaming report.

---

## 📦 Model Versions Overview

| Version | Embedding Model | Dataset Size | Class Balance        | Best Classifier  | F1-Score | Final Risk Score |
| ------- | --------------- | ------------ | -------------------- | ---------------- | -------- | ---------------- |
| v1.0    | MiniLM          | 4,968        | SMOTE (Oversampling) | LightGBM (Tuned) | 0.6699   | 43.79            |
| v2.0    | MPNet           | 4,968        | SMOTE (Oversampling) | LightGBM (Tuned) | 0.6555   | 43.55            |
| v3.0    | MiniLM          | 146,631      | SMOTE (Oversampling) | LightGBM (Tuned) | 0.6772   | 48.22            |
| v4.0    | MiniLM          | 146,631      | Undersampling        | LightGBM (Tuned) | 0.6996   | 16.40            |
| v5.0    | MPNet           | 146,631      | Undersampling        | LightGBM (Tuned) | 0.6940   | 13.01            |

---

## ✅ Key Findings & Insights

### 🧠 Embedding Model Comparison

- **MiniLM (v1.0, v3.0, v4.0)** consistently outperforms MPNet in overall F1-score and real-world risk detection, especially when trained with a large dataset.
- **MPNet (v2.0, v5.0)** gives close results in balanced scenarios but slightly underperforms MiniLM in nuanced CVSS class predictions.

### 📈 Data Scaling Effects

- Transitioning from ~5k entries (v1.0/v2.0) to 146k+ (v3.0+) resulted in significant performance improvements.
  - F1-Score increased from ~0.66 to ~0.69.
  - Classifier robustness, particularly for `Low` and `Critical` classes, improved notably.

### ⚖️ Class Balancing Strategy

- **Undersampling (v4.0, v5.0)** provided more stable and realistic severity class predictions compared to **SMOTE**.
- Lower final risk scores (16.40 in v4.0 and 13.01 in v5.0) demonstrate reduced over-prediction of `High`/`Medium` severities.

### 🏆 Best Performing Configuration

- **v4.0** (MiniLM + Undersampling + LightGBM) is the best-performing version overall.
  - Highest F1-Score (0.6996)
  - Balanced classification across all classes
  - Final Garak Risk Score significantly reduced to **16.40**

---

## 📌 Recommendation

> **Adopt `v4.0` as the production-ready model** for CVSS severity prediction.
>
> ✅ Best trade-off between accuracy, class fairness, and real-world severity interpretation.
> ✅ Significantly reduced overestimation of vulnerabilities in the Garak LLM test suite.

---

## 🔮 Future Enhancements

1. **Ensemble Modeling**Combine outputs from both MiniLM and MPNet-based models for even better robustness.
2. **Multi-modal Input Integration**Use additional CVE metadata (CWE IDs, vector strings, product names) along with descriptions.
3. **Temporal CVE Trends**Incorporate CVE publication year as a feature for context-aware training.
4. **Model Explainability**Integrate SHAP/LIME for transparency in CVSS label attribution.
5. **Feedback Loop**
   Periodically re-train using newly published CVEs and red-teaming reports.

---

## 📂 Folder Structure (for context)

```
data/
├── <version>/garak/
│   ├── garak.report.jsonl
│   ├── garak_report_flat.csv
│   └── garak_with_severity_historic.csv
├── <version>/nvd_data/
│   ├── all_nvd_cves.pkl
│   └── nvdcve-1.1-<YEAR>.json.gz
models/
└── <version>/best_cvss_classifier_historic.pkl
│   └── cvss_label_encoder_historic.pkl
```

---

## 📚 References

- [NVD CVE Database](https://nvd.nist.gov/)
- [Garak - LLM Red-Teaming](https://github.com/leondz/garak)
- [SentenceTransformers](https://www.sbert.net/)
- [LightGBM](https://lightgbm.readthedocs.io/)
