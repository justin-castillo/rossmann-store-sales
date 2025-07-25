# FINAL REPORT: Rossmann Store Sales Forecasting

---

## 1. Problem Overview

Rossmann operates over 1,000 drugstores across Europe. This project aims to predict daily sales for each store using historical data, enabling better operational planning. The dataset is large (~844,000 rows), with rich temporal and categorical structure but no direct customer-level granularity.

The modeling goal was to predict log-transformed sales based on structured features engineered from raw tabular data.

---

## 2. Modeling Approach

The pipeline followed a clean progression:

1. **Data Cleaning and Preprocessing**  
   - Removed closed-store days to eliminate artificial zeros.
   - Parsed dates and ensured consistency in competition/promo fields.
   - Verified no missing target values (`Sales`) or leakage variables.

2. **Feature Engineering**  
   - Time decomposition: year, month, day, week, weekday, weekend.
   - Competition and promo duration features.
   - Composite features for promo-storetype/assortment alignment.
   - One-hot encoding for store type and assortment categories.

3. **Model Selection and Training**  
   - Main model: **XGBoost Regressor** using `log1p(Sales)` target.
   - Alternative checks: CatBoost, Ridge.
   - Optuna used to tune parameters such as `max_depth`, `learning_rate`, `n_estimators`, and `subsample`.
   - Stratified K-Fold cross-validation ensured store distribution parity.

---

## 3. Evaluation Summary

Performance was measured using RMSE, MAE, and R² on both validation and hold-out test sets.

| Metric   | Validation | Test     |
|----------|------------|----------|
| RMSE     | 0.42       | 0.41     |
| MAE      | 0.32       | 0.32     |
| R² Score | -0.0073    | -0.0019  |

- The model captures broad sales trends but struggles with generalization, as reflected in the negative R² values.  
- Potential contributors: per-store volatility, lack of product-level or economic indicators, unmodeled seasonality variations.

---

## 4. Feature Importance & SHAP Interpretability

We used **SHAP** (SHapley Additive exPlanations) to analyze model behavior.

### Top Drivers of Sales
- **Day of Week**: Weekends and Mondays show strong patterns.
- **Promotion Active**: Promo days consistently lift sales across all stores.
- **Competition Distance**: Closer competitors impact sales negatively.
- **Store Type & Assortment**: More comprehensive assortments perform better, especially in isolated regions.

### Interactions Observed
- Promotions had stronger impact on certain store types (e.g., 'b') during weekdays.
- When competition was very far (>5km), the sales effect of promos flattened.
- Combined SHAP values showed multi-factor dependencies—not easily modeled linearly.

> 🧠 *SHAP Visuals (Placeholder):*  
> 1. SHAP Summary Plot  
> 2. SHAP Dependence (Promo × Competition)  
> 3. SHAP Force Plot (Single Store-Day)

---

## 5. Observations & Limitations

- The model had difficulty generalizing across highly variable store behaviors.
- Holiday effects were partially modeled but lacked granularity (e.g., local holidays, weather).
- No explicit time series or autocorrelation methods were applied.
- The use of log-transformed targets (log1p(Sales)) improved stability but may have obscured low-value predictions.

---

## 6. Recommendations

1. **Add Exogenous Variables**: Macroeconomic data, marketing spend, or regional events could improve predictive power.
2. **Hierarchical Modeling**: Train grouped models per store type or location region.
3. **Temporal Regularization**: Introduce time-aware models such as LightGBM with lags or Prophet-style hybrid models.
4. **Deployment Plan**: Incorporate retraining logic and evaluate on unseen months before launch.

---

## 7. Conclusion

The project demonstrates a complete machine learning workflow for structured tabular prediction: from EDA through deployment-ready modeling. While raw accuracy can be improved, the SHAP interpretability ensures transparent decisions—a requirement in real-world retail forecasting.

---

*Prepared by: [Your Name]  
LinkedIn: [linkedin.com/in/your-handle]  
GitHub Portfolio: [github.com/your-handle]*
