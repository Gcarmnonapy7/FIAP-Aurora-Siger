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
pip install numpy pandas seaborn matplotlib scikit-learn jupyter
```

A `.venv` directory exists at the root — use it.

## Notebook structure

The notebook follows this sequence (cells must be run in order — later cells depend on earlier ones):

1. **Imports** — numpy, pandas, sklearn
2. **Data generation** — creates `df_non_anomaly` (970k rows, 7 telemetry columns) using `np.random.seed(42)`
3. **`setAbortadoToADataFrame(df)`** — applies pre-launch safety rules vectorized; adds `abortado` boolean column. Rules are based on safe ranges per sensor (see table below). Skips columns not present in the DataFrame.
4. **Benchmark cell** — compares loop vs vectorized performance; controlled by `SAMPLE_SIZE` constant
5. **EDA** — `heatmap_plot()` and `distribution_plot()` helpers
6. **Model stubs** — `DecisionTree` and `IsolationTree` classes (not yet implemented)
7. **1.2 Pseudocode / flowchart** — algorithm description in Portuguese
8. **Reflexão crítica** — essay in Portuguese (section 1.6)

## Telemetry safe ranges

| Column | Safe range | Abort condition |
|---|---|---|
| `temperatura_interna` | 18–26 °C | outside range |
| `temperatura_externa` | -65–125 °C | outside range |
| `integridade_estrutural` | 0 = intact | == 1 |
| `energy` | ≥ 60% | < 60 |
| `vibration` | 0.1–0.5 g | outside range |
| `tank_pressure` | 270–340 atm | outside range |
| `navegation` | 1 = active | == 0 |

## Key design decisions

- `integridade_estrutural` and `navegation` are Bernoulli-distributed, correlated with `tank_pressure` via a logistic function (failure probability rises when pressure exceeds 320 atm)
- The dataset is intentionally **not** split into anomaly/non-anomaly at generation time — the commented-out `df_anomaly` block and `pd.concat` are placeholders for future work
- `setAbortadoToADataFrame` mutates the DataFrame in-place (no return value)
- Vectorized operations are preferred over loops — the benchmark cell documents the ~200x speedup
