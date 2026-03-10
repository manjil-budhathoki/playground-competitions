# 📊 Kaggle Playground S6E3: Telco Customer Churn
**Metric:** Area Under the ROC Curve (AUC-ROC)

## 📈 Score Tracker & Leaderboard Progress

| Experiment | Submission File | Public LB Score | Improvement | Notes / Strategy Used |
| :--- | :--- | :--- | :--- | :--- |
| **01. Baseline** | `submission_lgbm_only.csv` | **0.91371** | - | Fast baseline using only LightGBM. Applied basic domain-specific feature engineering. |
| **02. Basic Ensemble** | `submission_ensemble.csv` | **0.91384** | +0.00013 | Combined LightGBM, XGBoost, and CatBoost. Used manual weight guessing. |
| **03. Optimized Blend** | `submission_optimized_blend.csv` | **0.91395** | +0.00011 | Used `scipy.optimize` to mathematically calculate the perfect ensemble weights. |
| **04. Original Data** | *Pending Submission* | *TBD* | *TBD* | Injected the original IBM Telco dataset to give the models more real-world examples. |

---

## 🧠 What I Have Learned So Far

### 1. The Golden Rule of AUC-ROC
Because this competition is evaluated on AUC-ROC, the model's ability to **rank** predictions is what matters. 
* I learned to **never use `.predict()`**, which outputs hard 0s and 1s. 
* Instead, I must always use `.predict_proba()[:, 1]` to output the continuous probabilities of a customer churning.

### 2. Domain-Specific Feature Engineering is King
Tree models (like XGBoost) are powerful, but they struggle to calculate mathematical relationships across columns. By understanding the Telco business model, I created features that drastically helped the models learn:
* **The "Hidden Fees" Feature:** Calculated `Tenure * MonthlyCharges` and subtracted it from `TotalCharges`. A large difference indicates extra fees/hardware costs, which strongly correlates with Churn.
* **Service Ecosystem:** Counted the total number of services a customer has (Streaming, Security, etc.). Customers deeply integrated into the ecosystem are less likely to leave.

### 3. Model Diversity Matters
Instead of just using one model, I learned how to process the data differently based on what each algorithm prefers:
* **LightGBM & XGBoost:** Fast, but prefer Categorical features to be numbers (`OrdinalEncoder`).
* **CatBoost:** Slower, but mathematically superior at handling categorical data if fed **raw text/strings**. 

### 4. Never Guess Ensemble Weights
In my basic ensemble, I manually assigned weights (e.g., 0.4, 0.3, 0.3). However, by using `scipy.optimize.minimize`, I let the math find the exact decimal weights that maximized my Out-Of-Fold (OOF) AUC. 
* *Insight:* XGBoost was performing exceptionally well on this specific dataset, and the optimizer caught this, giving it over 60% of the voting weight, which immediately boosted my Public LB score to **0.91395**.

---

## 🚀 Next Steps to Increase Rank
1. **Original Dataset Injection:** Submit the results of the model trained with the original IBM Telco dataset concatenated to the synthetic training data.
2. **Hyperparameter Tuning:** Use `Optuna` to find the mathematically optimal `learning_rate`, `max_depth`, and `num_leaves` for LightGBM and XGBoost.
3. **Advanced Blending:** Try rank-blending instead of probability-blending, which is sometimes more robust for AUC optimization.