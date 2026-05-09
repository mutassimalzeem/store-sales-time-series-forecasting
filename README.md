# Store Sales Forecasting - End-to-End Time Series Forecasting Case Study

This repository documents a portfolio-ready forecasting project for Kaggle's **Store Sales - Time Series Forecasting** competition. The goal is to forecast daily sales across store and product-family combinations using historical demand, store metadata, holidays, oil prices, and carefully designed time-series features.

The project is intentionally framed as more than a notebook: it is a public case study covering business framing, relational data integration, leakage-safe feature engineering, time-aware validation, model comparison, error analysis, and a production-minded deployment plan.

## Project Thesis

Retail forecasting is not only a modeling problem. It is a data architecture problem: a useful model must combine sales history, store metadata, oil prices, holidays, promotions, and calendar effects without leaking future information.

## Project Highlights

- Relational integration across sales, stores, oil, holidays, transactions, and submission files
- Leakage-safe lag and rolling feature design
- Time-based validation with RMSLE as the guiding metric
- Baseline-first modeling philosophy before gradient boosting
- Error analysis by date, store, family, holiday period, and zero-sales behavior
- Public GitHub documentation structured like a professional case study
- Deployment plan for turning the notebook into a scheduled forecasting pipeline

## Repository Structure

```text
store-sales-time-series-forecasting/
├── README.md
├── CASE_STUDY.md
├── PLANNING.md
├── ARCHITECTURE.md
├── requirements.txt
├── data/
│   ├── raw/
│   ├── processed/
│   └── external/
├── notebooks/
├── docs/
├── reports/
│   └── figures/
├── outputs/
│   ├── submissions/
│   └── models/
├── assets/
│   └── diagrams/
└── project_management/
```

This scaffold is documentation-first. Source code and notebooks can be added later as the project moves from planning to implementation.

## Dataset

Competition: [Store Sales - Time Series Forecasting](https://www.kaggle.com/competitions/store-sales-time-series-forecasting)

Expected files:

| File | Purpose | Modeling Notes |
|---|---|---|
| `train.csv` | Historical sales by date, store, and family | Main supervised training data |
| `test.csv` | Future rows requiring predictions | Must receive the same feature pipeline as training data |
| `stores.csv` | Store city, state, type, and cluster | Static store-level features |
| `oil.csv` | Daily oil price | Economic signal with missing-value handling |
| `holidays_events.csv` | National, regional, local holidays and events | Requires careful aggregation before joining |
| `transactions.csv` | Store-level transaction counts | Strong EDA signal, but only model-safe if available at prediction time |
| `sample_submission.csv` | Submission format | Defines required output schema |

Raw Kaggle data should not be committed unless licensing and file-size rules allow it.

## Methodology

1. **Data understanding**: inspect schemas, date ranges, keys, missing values, and target behavior.
2. **Relational integration**: join sales rows to stores, holidays, oil, and optional transaction signals.
3. **Feature engineering**: create deterministic calendar, holiday, oil, lag, rolling, and Fourier features.
4. **Validation**: use date-based holdout and optional expanding-window validation. No random shuffle.
5. **Modeling**: compare naive, moving-average, linear/Ridge, LightGBM, XGBoost, and/or CatBoost models.
6. **Evaluation**: optimize RMSLE, clip predictions at zero, and compare validation and leaderboard scores.
7. **Error analysis**: explain residuals by family, store, date, holiday periods, and zero-sales behavior.
8. **Deployment plan**: outline batch inference, monitoring, retraining, and reporting paths.

## Success Criteria

- A clean notebook or notebook series with readable markdown narration
- A reproducible feature engineering pipeline for train and test data
- Leakage-safe validation with clearly documented dates
- At least one strong gradient boosting model
- Error analysis that explains where the model succeeds and fails
- Final `submission.csv`
- Public documentation that tells a complete story: business problem -> data -> features -> validation -> models -> errors -> submission -> deployment

## Current Status

| Area | Status |
|---|---|
| Repository planning | Drafted |
| Data dictionary | Drafted |
| Architecture documentation | Drafted |
| Leakage strategy | Drafted |
| Validation strategy | Drafted |
| EDA notebook | Not started |
| Feature pipeline | Not started |
| Modeling experiments | Not started |
| Final submission | Not started |

## Key Documents

- [CASE_STUDY.md](CASE_STUDY.md): long-form technical write-up outline
- [PLANNING.md](PLANNING.md): project roadmap, backlog, assumptions, and checklist
- [ARCHITECTURE.md](ARCHITECTURE.md): system design, data flow, deployment, and monitoring plan
- [docs/data_dictionary.md](docs/data_dictionary.md): source files, columns, and modeling use
- [docs/leakage_prevention.md](docs/leakage_prevention.md): leakage rules and review checklist
- [docs/validation_strategy.md](docs/validation_strategy.md): holdout and expanding-window evaluation design
- [docs/deployment_plan.md](docs/deployment_plan.md): production-style forecasting plan

