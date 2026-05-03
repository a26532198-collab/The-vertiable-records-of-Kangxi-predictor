# 《The Veritable Records of Kangxi: Word Analysis for Crisis Prediction with Machine Learning》

# **[Abstract]**

**This research utilizes the Qing Shilu (Veritable Records of the Qing) database of the Kangxi era (1661–1722) from the National Institute of Korean History to investigate whether word frequency patterns extracted from the 62-year reign of the Kangxi Emperor correlate significantly with periods of national crisis. Logistic Regression and Lasso classification models were applied to preprocessed text data, and Leave-One-Out Cross-Validation (LOO-CV) was employed to address small sample size issues. The analysis revealed that Model 1, utilizing the top 50 crisis-correlated terms as features, achieved an accuracy of 91.9% and an AUC-ROC of 0.989, demonstrating superior predictive power. These findings suggest that linguistic patterns in the Shilu serve as verbal indicators of national crisis and demonstrate the broad potential for applying Digital Humanities methodologies to East Asian historical studies.**

## **Keywords: Qing Shilu, Kangxi Emperor, Digital Humanities, Text Mining, Crisis Prediction, Logistic Regression, PCA**

---

## Data Visualization
<img width="3367" height="3183" alt="kangxi_light_model1" src="https://github.com/user-attachments/assets/060cd404-8d69-4296-ac4f-7e5123f33dd9" />
An infographic of Model 1 analysis result.

---

# Crisis Prediction from the Veritable Records of Kangxi: A Word Frequency Analysis

> A digital humanities study testing whether classical Chinese word frequency patterns in the Qing Shilu (1661–1722) can identify and predict periods of national crisis during the Kangxi Emperor's reign.

![Model 1 Result Infographic](assets/model1_infographic.png)

---

## Abstract

This research utilizes the Qing Shilu (Veritable Records of the Qing) database of the Kangxi era (1661–1722) from the National Institute of Korean History to investigate whether word frequency patterns extracted from the 62-year reign of the Kangxi Emperor correlate significantly with periods of national crisis. Logistic Regression and Lasso classification models were applied to preprocessed text data, and Leave-One-Out Cross-Validation (LOO-CV) was employed to address small sample size issues. The analysis revealed that Model 1, utilizing the top 50 crisis-correlated terms as features, achieved an accuracy of 91.9% and an AUC-ROC of 0.989, demonstrating superior predictive power. These findings suggest that linguistic patterns in the Shilu serve as verbal indicators of national crisis and demonstrate the broad potential for applying Digital Humanities methodologies to East Asian historical studies.

**Keywords**: Qing Shilu · Kangxi Emperor · Digital Humanities · Text Mining · Crisis Prediction · Logistic Regression · PCA

---

## Research Question

This study begins not from a known historical case, but from a methodological question:

> *"Can word frequency patterns alone identify periods of historical crisis in classical Chinese historical records?"*

Framing the question this way deliberately precedes any specific historical narrative, avoiding **post hoc case selection bias** — the risk that a researcher chooses a case because its crises are already well-known, then constructs a hypothesis tailored to that case.

The Kangxi reign was selected on **ex-ante** criteria — long timespan, recording consistency under a single monarch, diverse event types, and database accessibility — rather than because of its famous events (Three Feudatories Revolt, Galdan Campaigns, etc.). In other words, the *Veritable Records of Kangxi* serves here as a **test corpus** for a method, not as a confirmation case for a thesis.

---

## Data

- **Source**: [National Institute of Korean History — Qing Shilu Database](http://db.history.go.kr/)
- **Collection**: 759 monthly CSV files via web scraping (Kangxi 1–61, i.e., 1661–1722)
- **Initial matrix**: 62 years × 35,171 unique terms
- **Final analysis matrix**: 62 years × 2,695 features (after preprocessing)
- **Crisis labels**: 28 years (45.2%) crisis / 34 years (54.8%) non-crisis  
  → **Chance Level = 54.8%**, used as the minimum threshold for any model claim

### Four Crisis Phases (Labeling Schema)

| Phase | Years | Key Events |
|-------|-------|-----------|
| ① Coastal Evacuation & Oboi Regency | 1661–1669 | Coastal removal edict (遷界令), Oboi's autocracy, fall of Oboi |
| ② Three Feudatories & Taiwan Conquest | 1673–1683 | Wu Sangui revolt, Chahar revolt, Penghu naval battle |
| ③ Albazin War & Galdan Campaigns | 1685–1697 | Russian frontier conflict, Treaty of Nerchinsk, Battle of Jao Modo |
| ④ Rites Controversy & Tibet Crisis | 1705–1721 | Chinese Rites Controversy, deposition of Crown Prince Yinreng, Lhasa expeditions |

Long-running phases were chosen over isolated event-years because Shilu language reflects sustained political-military tension rather than single-day shocks.

---

## Methodology

### Preprocessing Pipeline

759 monthly CSVs → Yearly aggregation → N-gram tokenization (bi-/tri-grams) for un-spaced classical Chinese → Removal of 171 function words (虛辭) → NULL → 0 (sparse matrix: 78–96% zeros) → TF-IDF transformation → Standardization → Removal of leakage-prone derived columns (rare-word ±)

---


### Three Competing Models

| Model | Dimensionality Reduction | Features | Rationale |
|-------|--------------------------|----------|-----------|
| **Model 1** | Top-50 by Pearson correlation | 50 | Direct interpretability of crisis-signaling words |
| Model 2 | TF-IDF + PCA(20) | 20 PCs | Latent structure of full vocabulary |
| Model 3 | Rare-word filter + TF-IDF + PCA(20) | 20 PCs | Noise control via low-frequency removal |

All models use **L2-regularized Logistic Regression** and **L1-regularized Lasso**, evaluated under uniform Leave-One-Out Cross-Validation (62 folds).

### Validation Strategies

- **LOO-CV** for primary performance estimates
- **Nested LOO-CV** to detect feature-selection leakage in Model 1
- **Sensitivity analysis** shifting crisis-onset boundaries by ±2 years to test label robustness

---

## Results

### Model Comparison (LOO-CV)

| Metric | **Model 1** | Model 2 | Model 3 |
|--------|-------------|---------|---------|
| Accuracy | **0.919** | 0.677 | 0.677 |
| AUC-ROC | **0.989** | 0.729 | 0.742 |
| F1-score | **0.906** | 0.655 | 0.655 |

📊 [Figure 1 — Model comparison](assets/fig1_model_comparison.png)

The result is striking: a simple correlation-based feature selection (Model 1) substantially outperforms PCA-based latent-structure models. This suggests that crisis signals in the Shilu are concentrated in a **small set of direct lexical markers**, not diffused across the full vocabulary distribution.

### Crisis-Signaling Keywords (Model 1)

| Direction | Top Keywords | Interpretation |
|-----------|--------------|---------------|
| Positive (crisis) | 人心 · 達賴 · 殺 · 翰林院 · 鼓舞 · 休允 | Popular sentiment, Tibetan affairs, punitive violence, mobilization |
| Negative (stability) | 還宮 · 應行 · 百姓 · 城隍 | Imperial return, routine administration, civilian governance |

📊 [Figure 2 — Keyword contributions](assets/fig2_keyword_contribution.png)

These terms align with documented historical patterns: `人心` (popular sentiment) tracks legitimacy crises during the Three Feudatories Revolt; `達賴` (Dalai) marks late-Kangxi Tibetan campaigns; `殺` (kill) indexes punitive military mobilization; `還宮` (return to palace) inversely signals the closure of imperial expeditions and the resumption of routine governance. The model thus does not merely classify — it surfaces the **administrative-political vocabulary** through which the Qing court textually marked crisis itself.

### Honest Re-evaluation: Nested LOO-CV

The headline AUC of 0.989 must be qualified. Because Top-50 feature selection in Model 1 was performed **outside** the cross-validation loop, label information leaked into feature selection, optimistically biasing the estimate. Under **nested LOO-CV**, where feature selection is repeated within each fold, AUC drops to **0.693**.

This drop is reported transparently rather than concealed. Two facts mitigate but do not erase the concern:

1. **Feature stability**: Mean Jaccard similarity of Top-50 word sets across folds = **0.743**. Core terms (`一千`, `遣`, `進發`, `軍務`, `赴`, `遵行`, `曾經`) appear in *every* fold. The signal is not a one-fold artifact.
2. **Mid-reign concentration**: The drop is driven primarily by transition years at the edges of the reign (early regency, late succession crisis). Middle-period crises remain well-identified.

📊 [Figure 3 — Nested CV comparison](assets/fig3_nested_cv.png)

### Sensitivity Analysis: Crisis Boundary Shifts

To test whether results depend on the exact crisis-year labeling, crisis-onset years were shifted by ±2 years while end-years were held fixed. Model 1 AUC remained between **0.929 and 1.000** across all shift conditions, confirming that the predictive signal does not hinge on a particular labeling decision. PCA-based models showed larger fluctuations, consistent with their reliance on diffuse latent structure rather than concrete lexical markers.

📊 [Figure 4 — Sensitivity analysis](assets/fig4_sensitivity.png)

---

## Repository Structure

. ├── docx/ # Full paper (Korean, .docx) ├── mining/ # Analysis notebooks │ ├── 01_preprocessing.ipynb │ ├── 02_data_modeling.ipynb │ └── 03_compensate.ipynb # Nested CV + sensitivity analysis ├── crawler_scrapper/ # Web scraper for Qing Shilu DB ├── data/ │ ├── raw/ │ │ └── 1661 - 1722/ # Raw monthly CSVs (gitignored) │ └── processed/ # Yearly frequency matrix ├── assets/ # Figures and infographics ├── requirements.txt ├── LICENSE └── README.md


---

## Reproduction

```bash
# Install dependencies
pip install -r requirements.txt

# Run notebooks in order
jupyter notebook mining/

---

Raw data is collected directly from the National Institute of Korean History (not redistributed here due to source licensing). The crawler_scrapper/ directory contains the scraper used; expect ~759 monthly files.

## **Limitations**
This is a first-pass exploration, not a finalized claim:
Coarse temporal resolution. Yearly aggregation smooths out month-to-month crisis dynamics. Future work should test monthly or quarterly granularity.
p ≫ n problem. With 2,695 features against 62 samples, overfitting risk is structural, not avoidable. Mitigations (rare-word removal, Top-K selection, PCA, LOO-CV, nested CV) reduce but cannot eliminate it. Expanding to other reigns (Yongzheng, Qianlong) is the natural next step for sample augmentation.
Quantitative findings need qualitative re-reading. Statistical association between a term and crisis labels does not equal historical meaning. Each surfaced keyword should be checked against the original Shilu entries it appears in.
Numeric tokens (二十一, 三十九) appear in the Top-50 but resist clean historical interpretation; they may reflect troop counts, dates, or document conventions rather than crisis content. These were retained statistically but excluded from the historical reading.

## **Related Work**
A complementary unsupervised analysis on the same dataset — FP-Growth association rule mining for monthly co-occurrence patterns — is maintained in a separate repository:
🔗 Kangxi-MBA-Association-Rule-Mining

Where this study asks "can a model predict crisis years?", the sister project asks "which classical Chinese terms tend to co-occur within a month?" — supervised prediction and unsupervised pattern discovery applied to identical source data.

## **License**
- Code: [MIT License](LICENSE)
- Data: The raw data is sourced from the [National Institute of Korean History](http://db.history.go.kr/).
- Analysis Content: [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

## Citation

If you use this code or data in your research, please cite:

[김형민] (2026). 청성조실록 한문 로지스틱회귀 분석.
[https://github.com/a26532198-collab/The-vertiable-records-of-Kangxi-predictor]

