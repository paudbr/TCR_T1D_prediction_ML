# Decoding T1D Autoreactive TCRs with Protein Language Models

This repository contains a comparative machine learning pipeline designed to distinguish **Type 1 Diabetes (T1D) autoreactive T-cell receptors (TCRs)** from viral-specific TCRs. The project explores the intersection of immunology and deep learning, specifically comparing high-dimensional embeddings from **ESM-2** against classical bioinformatics encoding schemes.

## Project Overview

Predicting TCR specificity is a "holy grail" in personalized medicine. This study focuses on the challenges of **model generalization** across different clinical datasets (VDJdb and McPAS-TCR) and the detection of biochemical signatures of autoimmunity in the CDR3 regions of paired $\alpha:\beta$ chains.

### Key Features:
* **Domain Pooling:** Integration of multiple TCR databases to mitigate "laboratory-specific" bias.
* **PLM Embeddings:** Utilization of **ESM-2 (320-dimensional)** to capture protein "semantics".
* **Classical Baseline:** Benchmarking against **BLOSUM50** positional encoding.
* **Leave-One-Antigen-Out (LOAO):** Rigorous validation to test cross-antigenic generalization (Insulin vs. GAD vs. ZnT8).

## Methodology & Workflow

1.  **Data Curation:** Merging VDJdb and McPAS-TCR, filtering for T1D-specific and Viral-specific (Control) paired sequences.
2.  **Encoding:** Parallel extraction of ESM-2 embeddings and BLOSUM50 vectors.
3.  **Model Battle:** Comparative analysis of **Logistic Regression (LASSO)**, **Random Forest**, **SVM**, and **XGBoost**.
4.  **Sanity Checks:** Analysis of CDR3 length bias to ensure the model learns biochemical motifs rather than sequence length heuristics.

## Key Results & Findings

### 1. Robust Performance & Generalization
By integrating multiple data sources, we improved the model's external reliability from a random guess (AUC ~0.53) to a robust **Mean AUC of 0.785** using ESM-2 and Logistic Regression.

### 2. Latent Space: ESM-2 vs. BLOSUM50
* **ESM-2:** Showed superior **linear separability** (AUC 0.79 with Logit), indicating a highly organized biological latent space.
* **BLOSUM50:** Performed best with non-linear models like **Random Forest (AUC 0.81)**, suggesting that fixed positional motifs are strong predictors for tree-based algorithms.

### 3. Antigenic Hierarchy (LOAO Analysis)
Our **Leave-One-Antigen-Out** validation revealed a fascinating asymmetric generalization landscape:
* **GAD65 / ZnT8 (AUC 0.72 - 0.78):** Strong cross-prediction, indicating a **shared biochemical framework** for these autoantigens.
* **Insulin (AUC 0.50):** The model failed to generalize to Insulin when excluded from training, highlighting its **unique structural niche** and distinct molecular signature in T1D autoimmunity.

## Requirements & Usage

### Dependencies:
* `python >= 3.8`
* `transformers` (HuggingFace ESM-2)
* `scikit-learn`
* `biopython`
* `seaborn` & `matplotlib`

### How to run:
1. Clone the repository:
   ```bash
   git clone [https://github.com/your-username/T1D-TCR-Predictor.git](https://github.com/your-username/T1D-TCR-Predictor.git)
   ```
2. Open TCR_autoreactive_predict.ipynb in Google Colab or Jupyter Lab.
3. Ensure a GPU environment is enabled for efficient ESM-2 embedding extraction.

**Contact:** Paula de Blas Rioja (pau.dbr2002@gmail.com)
