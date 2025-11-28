# Exploratory Data Analysis and Predictive Modeling of Revenue

This report presents a summary of the exploratory data analysis (EDA) and predictive modeling performed on a real estate dataset, with the objective of predicting revenue.

## Data Description

The dataset contains **11,744 observations** and **23 variables**, including information on:

* Company and business unit (BU)
* Location (City, Region)
* Property characteristics (Program, Type, Total Area)
* Key dates (Reservation, Completion, TF)
* Financial information (Revenue, Margin, Outstanding Amount)
* Sales details (Sold By, Contract Type, Status)

## Data Cleaning and Preprocessing

The following steps were taken to prepare the data for analysis and modeling:

* **Removal of unnecessary columns**: `Unnamed: 22` and `Column1` were empty and removed.
* **Data type conversion**:

  * `Revenue` and `Margin` converted to numeric after removing commas and handling non-numeric values.
  * Date columns (`Date_TF`, `Date_Reservation`, `Date_Completion`, `Date_Verification`) converted to datetime format.
* **Handling missing values**:

  * `Outstanding Amount` replaced with `0`, `Sold_By` replaced with `'UNKNOWN'`.
  * Other missing values imputed using median or forward-fill (`ffill`).
* **Zero-value handling**:

  * **6,748** entries in `Total_Area` were zero, indicating possible data entry issues or unmeasured properties.
* **Target variable transformation**:

  * Applied `log1p` to `Revenue` to normalize a highly skewed distribution.

## Exploratory Data Analysis

Key findings from the exploratory analysis include:

* **Revenue distribution**: Right-skewed with extreme values → logarithmic transformation justified.
* **Correlation analysis**:

  * Weak correlations among numeric variables.
  * `Total_Area` and `Margin` show the highest correlation with `Revenue`.
* **Categorical variables**:

  * Most properties sold are **apartments**.
  * Sales are concentrated in the **Casablanca-Settat** region.

## Predictive Modeling

### Linear Regression (Baseline Model)

A simple linear regression model was used as a baseline.

* **Performance**:

  * MSE: `9.622 × 10¹⁵`
  * R²: `-0.0003`

> A negative R² indicates the model performs worse than the mean → relationships in the data are not linear.

### XGBoost Regressor

A more advanced model, **XGBoost**, was trained with tuned hyperparameters.

* **Performance**:

  * RMSE: `98,474,599.13`
  * R²: `-0.0081`

> The negative R² persists despite model complexity, indicating high difficulty in predicting revenue with the available features.

* **Feature importance**:

  * Most important features: `Total_Area`, `Margin`, `Outstanding Amount`.

## Conclusions

* **Data quality issues**: Numerous missing or zero values in key variables.
* **Prediction complexity**: Low R² scores → available variables explain only a small fraction of revenue variance.
* **Non-linearity**: Confirmed by poor linear regression performance.
* **XGBoost performance**: Negative R² unexpected for this model type. Possible reasons include:

  * Insufficient feature engineering
  * Missing explanatory variables (e.g., market conditions)
  * Potential data leakage (e.g., `Margin` derived from `Revenue`)
