# Validation Strategy

The project uses time-aware validation because the task is to forecast future sales. Random validation splits are invalid because they allow future patterns to influence the training set and can overstate model quality.

## Metric

Primary metric: RMSLE

Implications:

- Predictions must be non-negative.
- Under-prediction and over-prediction are penalized on a log scale.
- Large absolute errors on high-sales rows are compressed compared with RMSE.
- Zero-sales behavior matters because `log1p(0)` is valid and common in sparse store-family combinations.

## Primary Holdout Split

Use a validation period near the end of the training data.

Requirements:

- Training dates must be strictly before validation dates.
- Validation period should resemble the test horizon where possible.
- Validation start and end dates must be recorded in every experiment.
- Feature generation must preserve the temporal boundary.

Experiment log fields:

| Field | Description |
|---|---|
| Run | Experiment ID |
| Date | Date experiment was run |
| Model | Model family |
| Feature set | Short feature description |
| Validation split | Date range |
| RMSLE | Validation score |
| Notes | Key assumptions or changes |

## Optional Expanding-Window Validation

Expanding-window validation tests robustness across multiple forecast periods.

Pattern:

```text
Fold 1: train [start -> t1], validate [t1+1 -> t2]
Fold 2: train [start -> t2], validate [t2+1 -> t3]
Fold 3: train [start -> t3], validate [t3+1 -> t4]
```

Use when:

- The single holdout score seems unstable
- The model is sensitive to holiday periods
- Leaderboard score differs strongly from validation

## Optional Sliding-Window Validation

Sliding-window validation uses a fixed-size training period and moves through time.

Use when:

- Recency is expected to matter strongly
- Older data may be less relevant
- Memory or training time must be controlled

## Model Selection Rules

Choose the final model using:

- Validation RMSLE
- Stability across folds if available
- Simplicity and reproducibility
- Error behavior on holidays and zero-sales rows
- Feature importance interpretability
- Public leaderboard gap after submission

Do not select a model only because it improves public leaderboard score once.

## Reporting Template

| Model | Feature Set | Validation Period | RMSLE | Notes |
|---|---|---|---|---|
| Naive baseline | Previous week / average | TBD | TBD | First score to beat |
| Linear/Ridge | Date + store + family | TBD | TBD | Deterministic baseline |
| LightGBM | Date + store + family + holidays | TBD | TBD | First boosting model |
| LightGBM/XGBoost/CatBoost | Full lag + rolling feature set | TBD | TBD | Main candidate |

