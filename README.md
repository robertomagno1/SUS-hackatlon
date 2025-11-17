# **1st Place – Best Report Award at Stats Under the Stars 8 (SUS8)**
High-precision AML detection via **PCA-guided feature selection**, **log-normal synthetic augmentation**, and **XGBoost**.

---

## 📌 Problem Overview

Financial institutions must detect **rare laundering events** hidden within tens of thousands of legitimate transactions. In the SUS8 challenge, the goal was to classify transactions as **fraudulent (Is Laundering = 1)** or **legitimate**, using only the provided tabular data.

The dataset contains **55 307 transactions**, with only **685 fraud cases (1.24%)**, creating severe class imbalance . Effective modeling required careful feature analysis, augmentation, and robust validation.

---

## 📊 Dataset Summary

Key characteristics of the data (from EDA) include:

* **No missing values**, clean schema, one self-loop transaction.
* **Extreme class imbalance**: 1.24% fraudulent transactions (685 out of 55 307) .
* **Critical features identified by PCA** explaining ~75% of variance:

  * Avg Stock Account To
  * Avg Stock Account From
  * Transaction Count
  * Amount Paid
    (each contributing roughly 0.50 importance) .

---

## 🧠 Methodology (Modular Overview)

### **1. Exploratory Data Analysis**

* Verified no nulls and consistent datatypes.
* Checked graph-like relationships; only one self-referential transaction.
* Identified imbalance + key skewed distributions.

---

### **2. Dimensionality Analysis – PCA**

PCA revealed that **four numerical features account for 75% of total variance**, guiding a minimal yet information-rich feature subset for modeling .

Methods used:

* Standardization
* PCA with explained-variance ranking
* Feature-importance selection

---

### **3. Synthetic Minority Augmentation**

To counter extreme imbalance, the team generated **2 050 synthetic fraud samples** using **log-normal distributions** fitted on minority-class feature densities (Amount Paid, Avg Stock From/To) .

Steps:

* Estimated empirical density per feature.
* Validated best-fit distributions (log-normal).
* Sampled synthetic rows preserving the minority’s statistical shape.
* Engineered **Transaction Count** as a new behavioral feature.

---

### **4. Preprocessing & Feature Engineering**

* Removed non-informative columns.
* Normalized skewed features (standard or softmax scaling).
* Factorized categorical variables.
* Constructed final modeling table of real + synthetic rows.

---

### **5. Modeling – XGBoost**

Chosen for its robustness on tabular and imbalanced data.

Methods applied:

* **Grid Search** over:

  * η ∈ {0.01, 0.05, 0.1, 0.2}
  * subsample ∈ {0.7, 0.8, 1.0}
  * γ ∈ {0.5, 1, 3, 5}
* **1 000 boosting rounds** with **early stopping = 20**.
* Evaluation focused on **precision, recall, and F1** of the fraud class rather than accuracy alone .

---

## 📐 Key Evaluation Metrics

Final performance on the test set:

|                      Metric |   Value |
| --------------------------: | ------: |
|               **AUC (ROC)** |  0.9980 |
|       **Balanced Accuracy** |  0.9978 |
| **Fraud Capture Rate @485** |  14.78% |
|        **Overall Accuracy** | 0.76511 |

> Metrics reflect the final XGBoost model with PCA-guided features + synthetic augmentation.

---

## 🗂 Repository Structure

```text
.
├── data/
│   ├── train/       # raw training data (not committed)
│   └── test/
├── references/      # curated academic literature
├── src/
│   ├── report.tex
│   └── main.ipynb   # initial prototype notebook
├── final.ipynb      # polished, commented notebook
├── report.pdf       # award-winning SUS8 final report
├── config/          # YAML configs for modular pipeline
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

make run        # clean → augment → encode → train → evaluate
```

Or explore interactively:

```bash
jupyter lab
```

---

## 🔄 Pipeline Summary

1. **clean_data.py** — type enforcement, anomaly removal, dataset validation
2. **augment.py** — fit log-normal; generate 2050 fraud samples
3. **encode.py** — factorize categoricals, scale numerics, engineer transaction count
4. **train.py** — XGBoost + grid search + early stopping
5. **evaluate.py** — compute AUC, balanced accuracy, fraud-capture metrics

Every step is parameterized through YAML configs for plug-and-play reproducibility.

---

## 🤝 Acknowledgements

We thank:

* **SIS – Società Italiana di Statistica** for organizing SUS8.
* **Rulex** for sponsoring the event and providing computational resources.

---

## 📄 License

Released under the **MIT License**.

---

## 📚 Citation

If you use this repository or methodology, please cite our SUS8 report:

```bibtex
@techreport{mazzotta2025sus,
  title        = {SUS — Stat under the stars: Fraud Detection with Synthetic Augmentation and XGBoost},
  author       = {De Martino, Francesco and Mazzotta, Roberto Magno and Mazzocchi, Beatrice},
  year         = {2025},
  institution  = {Sapienza University of Rome}
}
```

---

If you'd like, I can also create:
✅ A shorter README
✅ A more professional corporate-style README
✅ A PyPI-ready or paper-ready version
Just tell me the style you want!
