# Store Sales Forecasting Case Study

## 1. Executive Summary

This project forecasts daily sales for store and product-family combinations in a retail setting. Accurate forecasts can support inventory planning, staffing, replenishment, and promotion decisions by reducing understock and overstock risk.

The project combines multiple relational data sources, builds leakage-safe forecasting features, evaluates models with time-aware validation, and explains model behavior through residual analysis and feature importance.

Final results will be added after modeling:

| Metric | Value |
|---|---|
| Best validation RMSLE | TBD |
| Public leaderboard RMSLE | TBD |
| Best model | TBD |
| Final submission file | `outputs/submissions/submission.csv` |

## 2. Business Context

Retail demand forecasting is difficult because demand varies by store, product family, season, holiday schedule, local events, and broader economic context. Forecasting errors have direct business consequences:

- Under-prediction can cause stockouts and lost sales.
- Over-prediction can increase waste, holding cost, and markdown risk.
- Holiday and event periods can create sudden demand changes.
- Store-level variation means a global trend may not represent local behavior.

The business question is:

> How can a retailer forecast daily store-family sales using historical demand and external signals while avoiding future-information leakage?

## 3. Dataset and Relational Design

The competition provides several tables rather than one flat modeling file. This matters because the forecasting task depends on relational context.

| Source | Role |
|---|---|
| `train.csv` | Historical target values by date, store, and family |
| `test.csv` | Forecast horizon rows requiring predictions |
| `stores.csv` | Static store metadata: city, state, type, cluster |
| `oil.csv` | Daily oil price as a possible economic signal |
| `holidays_events.csv` | National, regional, local, transferred, bridge, and event days |
| `transactions.csv` | Store-level traffic signal for EDA and possible safe modeling use |

The primary modeling grain is:

```text
date x store_nbr x family
```

Data integration must preserve this grain. Holiday joins require special care because multiple holiday rows can exist for the same date and can accidentally duplicate sales rows if merged directly.

## 4. EDA Insights

This section will be filled after the exploratory notebook is complete.

Planned EDA questions:

- What are the train and test date ranges?
- Are all store-family combinations present each day?
- Which product families have the highest sales?
- Which product families frequently have zero sales?
- How do sales vary by day of week, month, and year?
- Do holidays, earthquake events, or transferred days affect demand?
- Does oil price have visible relationship with sales?
- Are transaction counts available for the forecast horizon?

Planned visuals:

- Total sales over time
- Sales by year and month
- Sales by day of week
- Sales by store type and cluster
- Product-family sales ranking
- Sales around the 2016 earthquake period
- Oil price trend and missing-value pattern

## 5. Feature Engineering Strategy

The feature strategy is built around reproducibility and leakage prevention. Any feature created for training must be available for validation and test using the same logic.

Feature groups:

| Group | Examples | Leakage Risk |
|---|---|---|
| Calendar | year, month, dayofweek, weekend, month end, payday | Low, known in advance |
| Store/product | store metadata, family, cluster, type | Low, static |
| Holiday | national/local/regional flags, transferred, bridge, work day | Low if dates are known and joins are aggregated |
| Oil | imputed oil price, lags, rolling averages | Medium; imputation must be documented |
| Sales lags | lag 1, 7, 14, 28, 30 by store-family | High; must only use past target values |
| Rolling sales | shifted rolling mean and standard deviation | High; target must be shifted before rolling |
| Fourier | weekly and annual seasonality terms | Low, deterministic from date |

## 6. Validation Strategy

Random train-test splitting is invalid because future sales would influence model evaluation. Validation must respect time order:

- Training dates come before validation dates.
- Lag and rolling features for validation use only historical target values available before each validation date.
- Predictions are clipped to non-negative values before RMSLE scoring.
- Validation dates are documented for every experiment.

Primary validation design:

| Split | Purpose |
|---|---|
| Date-based holdout | Main model selection |
| Expanding-window validation | Optional robustness check |
| Sliding-window validation | Optional recency-focused check |

## 7. Modeling Experiments

Modeling will start simple and become more complex only after the data pipeline is reliable.

Planned sequence:

1. Naive baseline: previous week sales or store-family average
2. Moving-average baseline
3. Linear or Ridge baseline
4. LightGBM first boosting model
5. XGBoost or CatBoost comparison
6. Hyperparameter tuning
7. Feature ablation
8. Final model selection

Experiment results will be tracked in [reports/experiment_log.csv](reports/experiment_log.csv).

## 8. Error Analysis

The final project should explain model behavior, not only report a score.

Planned analysis:

- Actual vs predicted sales
- Residuals over time
- Residuals by product family
- Residuals by store
- Residuals around holidays and earthquake events
- Zero-sales behavior
- Feature importance and business interpretation
- Validation score vs public leaderboard score

## 9. Final Submission

Final submission requirements:

- Apply the exact training feature pipeline to `test.csv`.
- Confirm all required feature columns exist.
- Confirm row order aligns with test IDs.
- Generate non-negative predictions.
- Save final file to `outputs/submissions/submission.csv`.
- Record public leaderboard score.
- Compare public score against validation score.

## 10. Deployment Plan

This notebook can evolve into a batch forecasting system:

- Scheduled ingestion of daily sales, stores, calendar, holiday, and oil data
- Feature tables generated on a fixed cadence
- Batch model inference for the forecast horizon
- Forecast output table consumed by dashboards or APIs
- Monitoring for data drift, missing inputs, and forecast error
- Retraining triggered by score degradation or scheduled cadence

See [docs/deployment_plan.md](docs/deployment_plan.md) for details.

## 11. Lessons Learned

To be completed after the final modeling phase.

Reflection prompts:

- Which relational sources mattered most?
- Which features improved validation?
- Where did the model fail?
- Did public leaderboard score match validation expectations?
- What would be improved in a production version?

