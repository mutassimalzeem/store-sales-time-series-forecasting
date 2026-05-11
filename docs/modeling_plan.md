# Modeling Plan

## Objective

Build reliable forecasting models using time-aware validation, beginning with simple baselines and progressing to gradient boosting.

## Modeling Philosophy

Start simple before using advanced models:

- Baselines reveal whether feature engineering is useful.
- Gradient boosting is strong for tabular forecasting.
- Interpretability matters for business forecasting.
- Transformer models are optional and should only come after a complete boosted-tree solution.

## Planned Models

| Model | Purpose |
|---|---|
| Previous-week baseline | First score to beat |
| Moving-average baseline | Smooth historical benchmark |
| Store-family average | Simple grouped benchmark |
| Linear/Ridge model | Deterministic baseline with features |
| LightGBM | First strong tabular model |
| XGBoost | Boosting comparison |
| CatBoost | Categorical handling comparison |
| Transformer/attention model | Optional bonus experiment |

## Tuning Plan

Tune only after a stable validation pipeline exists.

Candidate parameters:

- Learning rate
- Number of estimators
- Max depth or number of leaves
- Subsampling
- Column sampling
- Regularization
- Categorical handling strategy

## Final Model Selection

Select the final model based on:

- Validation RMSLE
- Stability across validation periods
- Error behavior on holidays and zero-sales rows
- Simplicity
- Training time
- Interpretability
- Public leaderboard gap

## Error Analysis Requirements

- Actual vs predicted plot
- Residuals over time
- Residuals by product family
- Residuals by store
- Residuals around holidays
- Zero-sales analysis
- Feature importance
- Business-language interpretation

