# Waze User Churn Prediction: Tree-Based Machine Learning Modeling

## 1. Overview
This project represents **Milestone 6** of the Waze User Churn Analytics initiative, following the **PACE (Plan, Analyze, Construct, Execute)** workflow. Building upon earlier exploratory data analysis, hypothesis testing, and baseline logistic regression modeling, this phase focuses on developing, tuning, and cross-comparing tree-based ensemble machine learning models (**Random Forest** and **XGBoost**) to predict monthly user churn on the Waze navigation app.

---

## 2. Business Understanding
* **Business Problem:** Monthly user churn—defined as users who uninstall or stop using the Waze app—directly impacts platform growth and engagement. Identifying at-risk users before they churn enables Waze's product and leadership teams to design targeted retention strategies.
* **Project Objective:** Build a high-recall binary classification model capable of identifying potential churners more effectively than a standalone logistic regression baseline, while uncovering the primary behavioral drivers behind user attrition.
* **Primary Evaluation Metric:** **Recall** was prioritized alongside precision, accuracy, and F1-score because failing to identify an at-risk user (false negative) carries a higher business cost than flagging a retained user for a retention incentive (false positive).

---

## 3. Data Understanding
* **Dataset Scope:** Analyzed ~14,999 Waze user behavioral records (14,299 complete records after removing missing target labels), containing usage frequency, driving distance, session counts, device type (`Android` vs. `iPhone`), and churn status (`retained` vs. `churned`, with an ~18% churn rate).
* **Feature Engineering:** Created six behavioral ratio and intensity features to capture user driving habits. All **6 engineered features ranked within the Top 10 most important predictors** in the final model:
  1. `km_per_hour` (mean driving speed)
  2. `percent_sessions_in_last_month` (recent engagement concentration)
  3. `total_sessions_per_day` (daily session frequency)
  4. `percent_of_drives_to_favorite` (routine/favorite destination reliance)
  5. `km_per_drive` (average distance per trip)
  6. `km_per_driving_day` (average distance per active driving day)
* **Data Splitting Strategy:** Implemented a **three-way split (Training, Validation, and Test sets)** rather than a standard two-way split. Performing model selection and hyperparameter tuning strictly on the validation set ensured an unbiased, realistic estimate of future performance on the unseen test set.

---

## 4. Modeling and Evaluation
Two tree-based ensemble models—**Random Forest** and **XGBoost**—were trained, tuned via cross-validation (`GridSearchCV`), and evaluated against the Milestone 5 Logistic Regression baseline:

| Model | Key Findings & Performance Highlights |
| :--- | :--- |
| **Logistic Regression (Milestone 5 Baseline)** | Served as the interpretable statistical baseline; lower recall (~9%) due to linear decision boundaries and sensitivity to multicollinearity/outliers. |
| **Random Forest Classifier** | Improved overall predictive metrics over logistic regression with minimal preprocessing, though slightly outperformed by gradient boosting. |
| **XGBoost Classifier (Champion Model)** | **Best overall fit:** Achieved a **17% recall score** on the test set—**nearly doubling the recall** of the Milestone 5 logistic regression model while maintaining comparable accuracy (~82%) and precision. |

* **Trade-off Analysis:** Tree-based ensemble models outperformed single linear models across all evaluation metrics and required less data preprocessing, though they offer lower direct interpretability ("black-box" nature) compared to logistic regression coefficients.

---

## 5. Conclusion & Recommendations
* **Key Conclusion:** While XGBoost nearly doubled churn recall compared to logistic regression and proved the strong predictive value of engineered behavioral features, an overall recall of **17%** indicates that aggregate user-level data alone is insufficient to consistently predict all user churn.
* **Recommendations for Second Iteration:**
  1. **Collect Drive-Level Telemetry:** Capture granular trip-level data for each user, including individual drive durations, timestamps, and geographic locations.
  2. **Track In-App Engagement:** Measure active user interactions within the app, such as how frequently drivers report or confirm road hazard alerts.
  3. **Monitor Route Diversity:** Track the monthly count of unique starting and ending locations per driver to distinguish routine commuters from exploratory drivers.
  4. **Iterate with Expanded Features:** Re-train the XGBoost pipeline once granular behavioral and geographic features are integrated into the data warehouse.
