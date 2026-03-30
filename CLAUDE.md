# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Aurora SIGER is a FIAP academic project (Fase 1, 2026) simulating an intelligent risk management system for spacecraft pre-launch telemetry. All work lives in a single Jupyter notebook: [Aurora_siger.ipynb](Aurora_siger.ipynb).

## Running the notebook

```bash
# Local (activate venv first)
.venv\Scripts\activate        # Windows
jupyter notebook Aurora_siger.ipynb

# Or open directly in Google Colab via the badge in the first cell
```

## Dependencies

```bash
pip install numpy pandas seaborn matplotlib scikit-learn plotly jupyter
```

A `.venv` directory exists at the root — use it.

## Notebook structure

The notebook follows this sequence (cells must be run in order — later cells depend on earlier ones):

1. **Imports** — numpy, pandas, sklearn
2. **Data generation** — creates `df_non_anomaly` (97k rows) and `df_anomaly` (3k rows) using `np.random.seed(42)`; concatenates into `init_dataframe` (100k rows, 7 telemetry columns + `anomaly` label)
3. **EDA** — `heatmap_plot()`, `distribution_plot()`, `boxplot_()`, `pairplot_data()`, and Plotly 3D scatter
4. **Isolation Forest from scratch** — `IsolationTreeNode`, `IsolationTree`, `MyIsolationForest` classes
5. **sklearn comparison** — side-by-side metrics (accuracy, precision, recall, F1, ROC AUC) with `sklearn.IsolationForest`
6. **Validator + launch decision** — `Validator` class, `ai_anomaly_check()`, `calculate_autonomy()`, `launch_decision()` pipeline
7. **1.2 Pseudocode / flowchart** — algorithm description in Portuguese
8. **1.4 Energia** — essay on energy management in space missions
9. **1.6 Reflexao critica** — essay on AI ethics in Portuguese

## Telemetry safe ranges

| Column | Safe range | Abort condition |
|---|---|---|
| `internal_temp` | 18–26 °C | outside range |
| `external_temp` | -65–125 °C | outside range |
| `structural_integrity` | 1 = intact | != 1 |
| `energy` | 60–100 % | < 60 |
| `vibration` | 0.1–0.5 g | outside range |
| `tank_pressure` | 270–340 atm | outside range |
| `critical_modules` | 1 = active | != 1 |

## Key design decisions

- `structural_integrity` and `critical_modules` are Bernoulli-distributed, correlated with `tank_pressure` via a logistic function (failure probability rises when pressure exceeds 320/300 atm for normal/anomaly data respectively)
- The dataset is split into `df_non_anomaly` and `df_anomaly` at generation time, with anomalies using deliberately shifted distributions (e.g. `tank_pressure` centred at 360 vs 305, `energy` at 40 vs 98)
- `anomaly` column: 0 = normal, 1 = anomaly
- `Validator` uses a rules dict (`RULES`) mapping column names to range/binary checks
- `launch_decision()` chains telemetry validation, AI anomaly scoring, and energy analysis into a single GO/NO-GO pipeline
- The Isolation Forest `_c()` method (average BST path length via Euler-Mascheroni) is intentionally defined in both `IsolationTree` and `MyIsolationForest` to keep each class self-contained
