# PGCB Bangladesh Hourly Grid Demand Forecasting

A machine learning project that forecasts hourly electricity demand in Bangladesh using historical PGCB data, weather information, and time-based features.

The goal is to support demand analysis, peak-load planning, and electricity grid management.

## Project Overview

| Item | Description |
|---|---|
| Dataset | [PGCB Hourly Generation Dataset — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/1175/pgcb+hourly+generation+dataset+(bangladesh)) |
| Target variable | `demand_mw` |
| Task | Hourly electricity demand forecasting |
| Weather data | Open-Meteo |
| Models evaluated | Linear Regression, Random Forest, LightGBM, XGBoost, and MLP |
| Best model in the reported results | Fine-tuned LightGBM Regressor |
| Final test \(R^2\) | 0.9740 |
| Final test MAPE | 2.14% |

## Project Pipeline

| Step | Description |
|---|---|
| 1. Data cleaning | Removed duplicate records, reviewed missing values, and handled demand outliers. |
| 2. Weather integration | Combined historical hourly weather data with the demand dataset using timestamps. |
| 3. Feature engineering | Created calendar features, peak-period indicators, lag features, and rolling statistics. |
| 4. Feature selection | Used Spearman correlation to remove highly correlated features, reducing the feature set from 26 to 16. |
| 5. Model comparison | Evaluated several regression models using validation data. |
| 6. Hyperparameter tuning | Tuned the selected LightGBM model using time-series cross-validation and randomized search. |
| 7. Model interpretation | Used SHAP and feature-importance analysis to examine which features influenced predictions. |

## Model Performance

The following table shows the reported **validation-set** results.

| Model | MAE (MW) | RMSE (MW) | MAPE (%) | R² | Training Time (s) | Inference Time (s) |
|---|---:|---:|---:|---:|---:|---:|
| **LightGBM Regressor** | **282.16** | **548.98** | **3.36** | **0.9544** | 2.50 | 0.0659 |
| Random Forest Regressor | 282.93 | 587.07 | 3.43 | 0.9478 | 82.78 | 0.0936 |
| XGBoost Regressor | 338.09 | 658.76 | 3.75 | 0.9343 | 3.47 | 0.0333 |
| Linear Regression | 373.22 | 699.34 | 4.46 | 0.9260 | 0.06 | 0.0025 |
| MLP Regressor | 658.87 | 908.39 | 6.15 | 0.8751 | 83.22 | 0.3897 |

### Final Test Results: Fine-Tuned LightGBM

| Metric | Result |
|---|---:|
| R² score | **0.9740** |
| Mean Absolute Error (MAE) | **241.89 MW** |
| Root Mean Squared Error (RMSE) | **353.13 MW** |
| Mean Absolute Percentage Error (MAPE) | **2.14%** |
| MAE as a percentage of mean test demand | **2.09%** |

These results indicate that the model performed well on the held-out test set. Performance on future data may differ from the reported results.

## Key Findings

- **Seasonal demand:** The analysis found higher electricity demand during the summer months than in winter, consistent with increased cooling needs.
- **COVID-19 period:** Demand patterns changed during the 2020 lockdown period compared with typical demand patterns.
- **Important features:** Recent demand values, including `demand_lag_1h` and `demand_lag_24h`, and time-based features such as `hour` were identified as useful predictors.
- **Model comparison:** LightGBM achieved the strongest validation results among the models listed above.

## Technologies Used

| Tool or library | Purpose |
|---|---|
| Python | Main programming language |
| Pandas and NumPy | Data processing |
| Matplotlib and Seaborn | Data visualization |
| Scikit-learn | Data preparation, evaluation, and model selection |
| LightGBM and XGBoost | Gradient-boosting models |
| TensorFlow / Keras | Multilayer Perceptron model |
| SHAP | Model interpretation |
| Requests | Retrieving weather data |
| Open-Meteo API | Historical weather information |
| Jupyter Notebook / Google Colab | Running the analysis |

## How to Run

### 1. Clone the repository

Replace the example URL with the actual URL of your GitHub repository.

```bash
git clone https://github.com/yourusername/pgcb-grid-forecasting.git
cd pgcb-grid-forecasting
```

### 2. Install the dependencies

Make sure Python is installed, then run:

```bash
pip install pandas numpy matplotlib seaborn requests scikit-learn lightgbm xgboost tensorflow shap openpyxl xlrd
```

### 3. Open the notebook

Open the project notebook in Google Colab or Jupyter Notebook.

### 4. Run the notebook

Run the notebook cells in order to perform data preparation, retrieve or load weather data, train and evaluate models, and generate visualizations.

**Note:** The notebook may require internet access to retrieve weather data from Open-Meteo. If the repository includes prepared data files, follow the notebook's instructions for using them.

## Dataset and Data Notes

- The target variable is `demand_mw`, representing hourly electricity demand in megawatts.
- Weather data is matched with the demand records by timestamp.
- Lag and rolling features use historical demand values and should be constructed without using future information.
- The dataset is time-dependent, so chronological evaluation is important to avoid training on future observations.
- Results depend on the data preparation steps, feature construction, and evaluation split used in the notebook.

## Project Limitations

- Historical patterns may not fully represent future changes in electricity use.
- Weather data and other input features may contain missing or imperfect observations.
- Unusual events, changes in grid infrastructure, and sudden demand shifts can reduce forecasting accuracy.
- The reported metrics are experimental results, not a guarantee of real-world performance.

## Acknowledgements

- **Dataset:** [UCI Machine Learning Repository — PGCB Hourly Generation Dataset (Bangladesh)](https://archive.ics.uci.edu/dataset/1175/pgcb+hourly+generation+dataset+(bangladesh))
- **Weather data:** [Open-Meteo](https://open-meteo.com/)

---

*This project was developed for educational and machine learning research purposes.*
