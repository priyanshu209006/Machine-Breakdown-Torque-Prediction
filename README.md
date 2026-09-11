# Machine Breakdown & Torque Prediction

Predictive maintenance and equipment load forecasting models built on the UCI AI4I 2020 dataset.

The project tackles two main tasks:

1. **Torque Load Regression**: Predicting motor torque demand based on temperature, speed, and tool wear.
2. **Failure Classification**: Detecting machine breakdown events across 5 failure modes (tool wear, heat dissipation, power failure, overstrain, random).

## Dataset

- **Source**: UCI AI4I 2020 Predictive Maintenance Dataset (`ai4i2020_predictive_maintenance.csv`)
- **Records**: 10,000 rows
- **Target Variables**:
  - `torque_nm` (Continuous, range: 3.8 – 76.6 Nm)
  - `machine_failure` (Binary, 339 failures / 3.39% baseline rate)
- **Input Features**: Air temperature [K], Process temperature [K], Rotational speed [rpm], Torque [Nm], Tool wear [min].

## Model Benchmarks

### 1. Torque Prediction (Regression)

Train/test split: 80/20 (`random_state=42`).

| Model                       | $R^2$         | MAE (Nm)       | RMSE (Nm)      |
| --------------------------- | --------------- | -------------- | -------------- |
| Linear Regression           | 0.757           | 3.69           | 4.90           |
| Random Forest (100 trees)   | 0.825           | 3.11           | 4.15           |
| **XGBoost Regressor** | **0.834** | **3.03** | **4.05** |

### 2. Breakdown Detection (Classification)

Stratified 80/20 split with XGBoost Classifier (`max_depth=4`, `lr=0.1`). Decision threshold tuned to `0.35` for higher minority class recall.

| Metric              | Normal | Failure | Overall         |
| ------------------- | ------ | ------- | --------------- |
| **Precision** | 0.99   | 0.81    | 0.98            |
| **Recall**    | 0.99   | 0.74    | 0.98            |
| **F1-Score**  | 0.99   | 0.77    | 0.98            |
| **ROC-AUC**   | —     | —      | **0.973** |

## Feature Importance

Feature contributions to failure prediction from the trained classifier:

| Feature             | Importance |
| ------------------- | ---------- |
| Torque              | 29.9%      |
| Rotational Speed    | 24.6%      |
| Tool Wear           | 18.8%      |
| Air Temperature     | 18.0%      |
| Process Temperature | 8.6%       |
