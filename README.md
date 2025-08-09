This project delivers a full-cycle machine learning workflow to forecast daily sales for Rossmann stores with a time‑aware **XGBoost** model. The pipeline cleans and engineers features, then evaluates on a final six‑week holdout to mirror deployment.

---

## Highlights

- **Objective:** Predict daily **log1p(Sales)** per store.
- **Data coverage:** 2013‑01‑01 → 2015‑07‑31; **844,338** open‑store rows; **1,115** stores.
- **Final test (log scale):** **RMSE 0.169**, **MAE 0.130**, **R² 0.833**.
- **Key drivers:** Promotions, competition distance, weekday and store effects.

---

## Repository Structure

```
.
├── 01_EDA.ipynb / 01_EDA.pdf          # Cleaning, EDA, feature engineering
├── 02_Modeling.ipynb / 02_Modeling.pdf# Time-based split, tuning, evaluation
├── data/
│   ├── train.csv                      # Kaggle Rossmann 'train' (place here)
│   ├── store.csv                      # Kaggle Rossmann 'store' (place here)
│   └── data_processed.csv             # Exported from EDA for modeling
├── exports/                           # Created by the modeling notebook
│   ├── xgb_final_model.joblib
│   ├── X_test_processed.csv
│   ├── y_test_processed.csv
│   └── test_predictions.csv
└── Rossmann_Final_Report_Pro.md       # Project report
```

---

## Quickstart

1. **Create an environment**
   ```bash
   python -m venv .venv && source .venv/bin/activate  # or use conda
   pip install -U pandas numpy scikit-learn xgboost optuna shap matplotlib
   ```

2. **Add data**
   - Place `train.csv` and `store.csv` into `data/`.

3. **Run EDA**
   - Open `01_EDA.ipynb` and run all cells.
   - It writes `data/data_processed.csv` used by modeling.

4. **Run Modeling**
   - Open `02_Modeling.ipynb` and run all cells.
   - It creates a chronological split (last six weeks = test), tunes XGBoost, evaluates once on the holdout, and saves artifacts under `exports/`.

---

## Results at a glance

- **Test performance (log scale):** RMSE **0.169**, MAE **0.130**, R² **0.833**.
- **Top signals:** Promo variables, CompetitionDistance, weekday and store effects.

---

## Reproduce Locally

- **EDA:** cleans data, builds engineered features (for example, `ActiveStoreCount`, promo interactions), applies log transforms, and exports `data_processed.csv`.
- **Modeling:** verifies leakage protection, encodes categoricals, tunes XGBoost, and exports the trained model and test artifacts.

---

## Predict with the Saved Model

```python
import joblib, pandas as pd, numpy as np
model = joblib.load("exports/xgb_final_model.joblib")
X = pd.read_csv("exports/X_test_processed.csv")  
pred_log = model.predict(X)
pred = np.expm1(pred_log)  # back-transform if training used log1p(Sales)
```

---

**Justin Castillo**  
[jcastillo.hotels@gmail.com](mailto:jcastillo.hotels@gmail.com)  
[GitHub](https://github.com/justin-castillo)  
[LinkedIn](https://www.linkedin.com/in/justin-castillo-69351198/)
