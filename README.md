# SUS-hackatlon

# SUS — Smashing Unfair Schemes (Fraud Detection)

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

````text
.
├── data/
│   ├── raw/            # original .csv (🔒 never committed)
│   └── processed/      # cleaned & augmented
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_pca.ipynb
│   ├── 03_synthetic_generation.ipynb
│   └── 04_modelling.ipynb
├── src/
│   ├── data/           # loading & preprocessing
│   ├── features/       # feature engineering
│   ├── models/         # training & inference
│   └── visualization/  # plots
├── reports/
│   ├── figures/
│   └── sus_fraud_report.pdf
├── environment.yml
├── Makefile
├── LICENSE
└── README.md
````

---

## ⚡️ Quickstart

```bash
# 1. Clone me (you know you want to)
git clone https://github.com/YOUR-USERNAME/sus-fraud-detection.git
cd sus-fraud-detection

# 2. Set up environment
conda env create -f environment.yml
conda activate sus

# 3. Run the full pipeline
make run          # executes: clean, augment, train, evaluate

# Or notebook your way through:
jupyter lab
```

---

## 🔧 Pipeline in 60 sec

1. **`clean_data.py`** — Removes anomalies, converts types, and laughs at nulls.
2. **`augment.py`** — Fits log‑normal distributions to key fraud features and spits out 2 050 new bad boys.
3. **`train.py`** — Runs grid search over `eta`, `subsample`, `gamma` (1 000 combos), with early stopping. Saves the best model to `models/xgb_best.json`.
4. **`evaluate.py`** — Prints and plots confusion matrix, ROC, PR curve, and dumps a `metrics.json`.

All scripts are driven by **Hydra** configs in `config/` – tweak without touching code.

---

## 📊 Result Highlights (test set)

| Metric     | Legit | Fraud |
|-----------:|------:|------:|
| Precision  | 0.92  | 0.84  |
| Recall     | 0.96  | 0.88  |
| F1‑score   | 0.94  | 0.86  |

<sub>*Numbers are illustrative; check the current `reports/metrics.json` for fresh results.*</sub>

---

## 🤝 Contributing

Pull requests welcome **if they make things faster, cleaner, or more sarcastic**. Open an issue first for major changes. Remember to run `pre-commit`.

---

## 📄 License

MIT — because life’s too short for restrictive licenses. See `LICENSE` for details.

---

## 🎓 Citation

If this repo saves your thesis/job, cite the accompanying report:

```bibtex
@techreport{mazzotta2025sus,
  title        = {SUS — Smashing Unfair Schemes: Fraud Detection with Synthetic Augmentation and XGBoost},
  author       = {De Martino, Francesco and Mazzotta, Roberto Magno and Mazzocchi, Beatrice},
  year         = {2025},
  institution  = {Sapienza University of Rome}
}
```

---

### 🙋‍♂️ Author — **Giuggini** (a.k.a. *Brutti Manzoni*)

I turn messy data into clean wins. **Hire me, let’s kill fraud together.**
