# Module 2: Analytics Pipeline - Titanic Survival Prediction

## Project Overview
This repository contains the end-to-end analytics and machine learning pipeline for the Titanic dataset. The objective of this module was to perform rigorous exploratory data analysis (EDA), engineer a robust preprocessing pipeline, handle class imbalances, and evaluate multiple classification models to predict passenger survival. A secondary regression task was also performed to predict ticket fares.

## Data Quality & Preprocessing
* **Missing Value Justifications:**
  * **Age (19.8% missing):** Imputed using the median to avoid the influence of extreme outliers present in the right tail.
  * **Embarked (0.2% missing):** Imputed using the most frequent value (mode) since only two records were missing.
  * **Deck (77.2% missing):** Dropped entirely from the dataset as the missingness exceeded the 75% threshold, making imputation unreliable.
* **Outlier Analysis:**
  * **Age:** Outliers exist on the higher end (passengers > 65 years old). These were retained as they represent valid, realistic passenger ages.
  * **Fare:** Extreme outliers exist on the high end (e.g., fares > 500). These were retained to accurately reflect the socio-economic disparity of the passengers, though they contribute to a heavy right-skew.

## Exploratory Data Analysis (EDA) Interpretations
1. **Age Distribution & Box Plot:** The age distribution is roughly symmetric, centered around the late 20s, with a noticeable spike in infants and toddlers. The box plot reveals a right-skewed tail with outliers representing elderly passengers.
2. **Fare Distribution & Box Plot:** The fare distribution is heavily right-skewed. The vast majority of passengers paid very low fares, while extreme outliers pull the mean significantly higher than the median.
3. **Survival Rate by Sex and Passenger Class:** Both sex and passenger class were massive factors in survival. Females across all classes survived at much higher rates than males, and 1st class passengers survived at much higher rates than 3rd class passengers.
4. **Pair Plot of Numeric Features:** `pclass` and `fare` serve as the strongest numeric separators for survival. Survivors clustered heavily at lower `pclass` values and higher `fare` values. 
5. **Standardization Check:** The `age` and `fare` columns were successfully standardized. The transformed variables maintain their exact original distributions but are correctly centered on a mean of 0 with a standard deviation of 1.

## Imbalance Handling Conclusion
The initial training set exhibited a moderate class imbalance, with roughly 61.7% of passengers in the 'not-survived' class and 38.3% in the 'survived' class. 

* The **Baseline** model achieved a solid Precision of 0.7812 but had a lower Recall of 0.7353, indicating it missed some actual survivors.
* Applying **`class_weight='balanced'`** adjusted the penalization for missing the minority class, which successfully improved Recall to 0.7500, but caused Precision to drop to 0.7612. 
* **SMOTE** generated synthetic data points for the minority class to create a balanced training split. This resulted in significant improvements across the board, achieving a Recall of 0.7794 and a Precision of 0.7910.

Overall, **SMOTE (Oversampling)** proved to be the most effective approach for this dataset. It achieved the highest F1 Score (0.7852), representing the best-balanced trade-off between successfully identifying true survivors (Recall) and minimizing false survivor predictions (Precision).

## Regression Sub-Task Conclusion
A multivariate linear regression model was trained to predict the `fare` based on the other available features. Looking at the residual plot, there is clear evidence of **heteroscedasticity**. The spread of the residuals is not random or uniform; instead, it forms a "funnel" shape where the error variance increases significantly as the predicted fare increases. This indicates that the standard linear model struggles to accurately predict higher-priced tickets due to the heavily right-skewed nature of the fare data.

## Final Model Comparison

### Final Model Comparison

| Model Category | Model Name | Accuracy | Precision | Recall | F1 Score | AUC | MAE | RMSE | R² | Adjusted R² |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Classification** | Logistic Regression | 0.809 | 0.7833 | 0.6912 | 0.7344 | 0.861 | - | - | - | - |
| **Classification** | Decision Tree | 0.7697 | 0.6901 | 0.7206 | 0.705 | 0.7541 | - | - | - | - |
| **Classification** | Random Forest (SMOTE) | 0.7933 | 0.7910 | 0.7794 | 0.7852 | 0.8520 | - | - |


## Deployment Recommendation
I recommend deploying the **Random Forest Classifier trained with SMOTE oversampling**. While the baseline models struggled with the moderate class imbalance of the Titanic dataset, the SMOTE-enhanced Random Forest achieved the highest overall F1 Score (0.7852). It successfully balanced the trade-off between identifying true survivors (Recall of 0.7794) and minimizing false predictions (Precision of 0.7910). Additionally, the ensemble nature of the Random Forest provides superior robustness against overfitting compared to a standalone Decision Tree, making it the most reliable choice for raw, unseen data.

## Artifacts
* `zepto_titanic_survival_pipeline.pkl`: The final `joblib` serialized pipeline containing the complete end-to-end preprocessing steps (imputer, scaler, one-hot encoder), SMOTE oversampling, and the tuned Random Forest estimator.