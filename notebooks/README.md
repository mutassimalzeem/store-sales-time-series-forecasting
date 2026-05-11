# Notebook Plan

This folder will contain the project notebooks. To keep the initial repository scaffold code-free, the executable notebooks are not created yet.

Planned notebook sequence:

| Notebook | Purpose |
|---|---|
| `01_eda_and_data_understanding.ipynb` | Data loading, schema checks, relational mapping, missing values, EDA visuals |
| `02_feature_engineering_and_validation.ipynb` | Calendar, holiday, oil, lag, rolling, Fourier features, validation split |
| `03_modeling_and_error_analysis.ipynb` | Baselines, gradient boosting, tuning, residuals, feature importance |
| `04_final_submission.ipynb` | Final model fit, test features, submission file, public score reflection |

Notebook writing rules:

- Write markdown before major sections.
- Explain decisions and implications after important plots.
- Keep debug output out of the final version.
- Track validation dates and RMSLE in the experiment log.
- Keep feature logic reproducible for train and test.

