# Risk Register

| Risk | Why It Matters | Mitigation | Status |
|---|---|---|---|
| Data leakage from lag or rolling features | Can produce unrealistic validation scores | Shift target before rolling and validate features manually | Open |
| Random validation split | Invalid for time-series forecasting | Use date-based holdout and expanding-window validation | Open |
| Transactions unavailable for future dates | May not be usable as a production feature | Use transactions mainly for EDA unless future-safe logic is defined | Open |
| Holiday merge duplication | Multiple holiday rows can duplicate sales rows | Aggregate holiday features before merging | Open |
| Oil price missing values | Missing economic signal can create noise | Use documented imputation and test impact | Open |
| High memory usage | Lag and rolling features can create large tables | Use efficient dtypes and create features selectively | Open |
| Leaderboard overfitting | Public score may encourage poor generalization | Trust validation design and keep experiment notes | Open |

