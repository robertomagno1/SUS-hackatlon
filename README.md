# 🏆 SUS8 — Anti Money Laundering Detection

**1st Place – Best Report Award at Stats Under the Stars 8 (SUS8)**
High-precision AML detection via **PCA-guided feature selection**, **log-normal synthetic augmentation**, and **XGBoost**.

---

## 📌 Problem Overview

Financial institutions must detect **rare laundering events** hidden among tens of thousands of legitimate transactions.
The SUS8 challenge required building a classifier for **“Is Laundering”**, using tabular transaction-level data with severe class imbalance.

The dataset contains **55 307 transactions**, with only **685 fraud cases (1.24%)**  — requiring careful strategy for variance reduction, augmentation, and robust boosting.

---

## 📊 Dataset Summary

* **No missing data**, consistent schema, one self-loop transaction.
* **Extreme imbalance**: 1.24% fraudulent (685 out of 55 307) .
* **PCA highlights 4 key features** explaining ~75% of total variance:

  * Avg Stock Account To
  * Avg Stock Account From
  * Transaction Count
  * Amount Paid
    (≈0.50 explained variance each) .

---

## 🧠 Methodology

### **1. Exploratory Data Analysis**

* Validated completeness and absence of missing values.
* Structural inspection of transaction graph.
* Initial outlier and distribution analysis.

---

### **2. Principal Component Analysis (PCA)**

PCA revealed a compact structure: only **four numeric features** were needed to explain 75% of variance.
Used for:

* Dimensionality guidance
* Feature significance ranking
* Identification of skewed variables

---

### **3. Synthetic Minority Augmentation**

To solve the imbalance, **2 050 synthetic fraud rows** were generated via:

* Empirical density estimation of minority-class features
* Best-fit log-normal distribution selection
* Sampling aligned to the “fraud profile” of Amount Paid and Avg Stock From/To 

Also engineered:

* **Transaction Count** (behavioral feature capturing account activity)

---

### **4. Preprocessing & Feature Engineering**

* Remove non-informative columns.
* Normalize skewed numeric features (standardization or softmax scaling).
* Factorize categorical variables.
* Integrate real + synthetic rows for training.

---

### **5. Modeling — XGBoost**

Hyperparameter grid search (1 000 rounds, early stopping = 20) over:

* η ∈ {0.01, 0.05, 0.1, 0.2}
* subsample ∈ {0.7, 0.8, 1.0}
* γ ∈ {0.5, 1, 3, 5}

Model selection prioritized **recall, precision, and F1 for the fraud class**, not raw accuracy.

---

## 📐 Key Evaluation Metrics

Final test-set results:

|                      Metric |   Value |
| --------------------------: | ------: |
|               **AUC (ROC)** |  0.9980 |
|       **Balanced Accuracy** |  0.9978 |
| **Fraud Capture Rate @485** |  14.78% |
|        **Overall Accuracy** | 0.76511 |

---

## 🗂 Repository Structure

```text
.
├── data/
│   ├── train/      
│   └── test/
├── references/
├── src/
│   ├── report.tex
│   └── main.ipynb
├── final.ipynb
├── report.pdf
├── config/
├── environment.yml
├── LICENSE
└── README.md
```

---

## ⚙️ Quickstart

```bash
git clone https://github.com/robertomagno1/SUS-hackatlon.git
cd SUS-hackatlon

conda env create -f environment.yml
conda activate sus

make run
```

Or open notebooks via:

```bash
jupyter lab
```

---

## 🔄 Pipeline Summary

1. **clean_data.py** — validation, type enforcement, outlier checks
2. **augment.py** — log-normal sampling → +2050 synthetic fraud rows
3. **encode.py** — factorize categoricals, scale numerics, engineer Tx count
4. **train.py** — XGBoost + grid search + early stopping
5. **evaluate.py** — final metrics (AUC, Balanced Accuracy, FCR@485)

---

## 🤝 Sponsors & Acknowledgements

A special thanks to the **SUS8 organizers and sponsors**:

### 🏢 **SIS – Società Italiana di Statistica**

📎 [https://www.sis-statistica.it](https://www.sis-statistica.it)

### 🧠 **Rulex**

📎 [https://www.rulex.ai](https://www.rulex.ai)

Your support, infrastructure, and organization were essential to the success of this project.

---

## 📄 License

Released under the **MIT License**.

---

## 📚 Citation

```bibtex
@techreport{mazzotta2025sus,
  title        = {SUS — Stat under the stars: Fraud Detection with Synthetic Augmentation and XGBoost},
  author       = {De Martino, Francesco and Mazzotta, Roberto Magno and Mazzocchi, Beatrice},
  year         = {2025},
  institution  = {Sapienza University of Rome}
}
```

