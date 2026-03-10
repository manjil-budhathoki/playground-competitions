# Playground Series S6E2: Predicting Heart Disease

## 🎯 Objective
The goal of this competition was to predict the likelihood of a patient having heart disease based on various medical attributes (age, cholesterol, heart rate, etc.).

*   **Task Type:** Binary Classification
*   **Evaluation Metric:** ROC-AUC
*   **Dataset:** Synthetic dataset based on the "Statlog (Heart)" dataset.

## 🏆 Results & Progression

| Approach | Model Used | CV Score (Local) | Public Score (Kaggle) | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Baseline** | LightGBM | 0.95329 | 0.95xxx | Initial run with raw features. |
| **Ensemble** | LGBM + XGB + CatBoost | **0.95519** | **0.954xx** | "Holy Trinity" blend. Fixed target encoding error. |

## 🛠️ Tech Stack
*   **Python** (Pandas, NumPy)
*   **Scikit-Learn** (StratifiedKFold, Preprocessing)
*   **Models:** LightGBM, XGBoost, CatBoost
*   **Environment:** Google Colab / Local VS Code

## 🧠 Key Learnings
1.  **Text Targets:** XGBoost requires target labels to be integers (`0`/`1`), whereas LightGBM can sometimes handle strings. We had to map `Absence -> 0` and `Presence -> 1` manually.
2.  **Ensembling:** Blending three distinct boosting algorithms (LightGBM, XGBoost, CatBoost) provided a significant boost over the single baseline model.
3.  **Cross-Validation:** Used `StratifiedKFold` (5 splits) to ensure our local validation score matched the Kaggle leaderboard.

## 📂 Project Structure
*   `01-eda.ipynb`: Exploratory Data Analysis (Viewing columns, distributions).
*   `02-baseline.ipynb`: Single LightGBM model.
*   `03-ensemble.ipynb`: The final blended solution.
*   `insights.md`: Detailed explanation of ROC-AUC and model choices.