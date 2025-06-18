# SUS-hackatlon

# SUS — Stats Under the Stars (Fraud Detection)

![Python](https://img.shields.io/badge/python-3.10%2B-blue) ![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

> **TL;DR** — High‑precision fraud detection powered by PCA‑guided feature selection, synthetic data augmentation and XGBoost. Reproducible, modular, and battle‑tested – *hire us already*.

---

## 🚀 Why You Should Care

- **Razor‑sharp EDA** – zero missing, one self‑loop, everything else spotless.  
- **Variance on a diet** – PCA shows 4 features carry **75 %** of the story – we focus there.  
- **Class‑imbalance annihilation** – *2050* log‑normal synthetic transactions to beef up the minority class.  
- **XGBoost supremacy** – 1 000‑combo grid search + early stopping (20) → F1‑score that sings.  
- **Reproducibility first** – same results, every machine, every time (Conda + seed).

---

## 🗂 Repo Layout

```text
.
├── data/
│   ├── train/            # original CSVs (🔒 do NOT commit)
│   └── test/      
├── references/
│   ├── 1. Scalable Semi-Supervised Graph Learning Techniques for Anti Money Laundering.pdf
│   ├── 2. Anti-money Laundering using Graph Techniques.pdf
│   └── 3. Anomaly_Detection_in_Graphs_of_Bank_Transactions_f.pdf
├── src/
│   ├── report.tex  # source code for latex report 
│   └──  main.ipynb # first notebook not commented
│ final.ipynb    #final notebook commented
│ report.pdf  # final report in pdf 
├── LICENSE MIT
└── README.md
```

---

## ⚡️ Quickstart

```bash
# 1. Clone repo
git clone https://github.com/robertomagno1/SUS-hackatlon.git
cd SUS-hackatlon

# 2. Install dependencies
conda env create -f environment.yml
conda activate sus

# 3. Run full pipeline
make run       # runs: clean → augment → train → evaluate

# Or explore step-by-step:
jupyter lab
```

---

## 🔧 Pipeline Overview (in 60 sec)

1. **clean\_data.py**

   * Drop anomalies, enforce dtypes, verify no nulls.
2. **augment.py**

   * Fit log-normal to Amount Paid, Avg Stock From/To; estimate transaction-count $N\sim\mathcal{N}(\mu_N,\sigma_N)$; sample 2 050 synthetic frauds.
3. **encode.py**

   * Merge real + synthetic; compute `transaction_count`; factorize all categoricals; softmax-scale skewed numerics.
4. **train.py**

   * Grid search η∈{0.01,0.05,0.1,0.2}, subsample∈{0.7,0.8,1.0}, γ∈{0.5,1,3,5} via `xgb.cv` (1 000 rounds, early\_stop=20).
   * Save best model to `models/xgb_best.json`.
5. **evaluate.py**

   * Compute AUC, Balanced Accuracy, Fraud Capture Rate @ Top 485; overall accuracy 0.76511; dump `metrics.json`.

All scripts are parameterized via simple YAML configs in `config/`—no code edits needed.

---

## 📊 Test-Set Results

|                      Metric |   Value |
| --------------------------: | ------: |
|               **AUC (ROC)** |  0.9980 |
|       **Balanced Accuracy** |  0.9978 |
| **Fraud Capture Rate @485** | 14.78 % |
|        **Overall Accuracy** | 0.76511 |

<sub>*Full metrics in `reports/metrics.json`.*</sub>

---

## 🤝 Contributing

Improvements welcome—especially if they make things faster, cleaner, or funnier.

1. Fork & branch.
2. Run `pre-commit`.
3. Submit a PR and reference an issue.

---

## 📄 License

Released under the **MIT License**—see `LICENSE` for details.

---

## 🎓 Citation

Please cite our hackathon report if you build on this work:

```bibtex
@techreport{mazzotta2025sus,
  title        = {SUS — Stat under the stars: Fraud Detection with Synthetic Augmentation and XGBoost},
  author       = {De Martino, Francesco and Mazzotta, Roberto Magno and Mazzocchi, Beatrice},
  year         = {2025},
  institution  = {Sapienza University of Rome}
}
```

