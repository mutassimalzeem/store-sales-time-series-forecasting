# Feature Engineering Plan

## Objective

Convert raw dates, relational metadata, and historical sales behavior into predictive features while keeping the pipeline reproducible and leakage-safe.

## Feature Groups

| Group | Planned Features |
|---|---|
| Calendar | year, month, day, dayofweek, weekofyear, quarter, weekend, month start, month end, payday |
| Store and product | store number, city, state, type, cluster, family |
| Interactions | store-family, store type-family, cluster-family |
| Oil | imputed price, lagged price, rolling averages |
| Holidays | holiday, national, regional, local, transferred, bridge, work day, grouped event |
| Lags | sales lag 1, 7, 14, 28, 30 by store-family |
| Rolling windows | shifted rolling mean and standard deviation over 7, 14, and 30 days |
| Fourier | weekly and annual smooth seasonality terms |

## Required Checks

- Confirm required source columns exist.
- Confirm row count does not change unexpectedly after joins.
- Confirm no duplicate `date x store_nbr x family` rows.
- Confirm lag features are shifted.
- Confirm rolling features are shifted before aggregation.
- Confirm train, validation, and test use the same feature columns.
- Confirm feature generation for test does not require test target values.

## Feature Backlog

| Feature | Priority | Notes |
|---|---|---|
| Basic calendar features | High | Deterministic and low risk |
| Store metadata | High | Static relational features |
| Holiday flags | High | Important for demand shocks |
| Sales lags | High | Strong forecasting signal; high leakage risk |
| Rolling sales stats | High | Strong forecasting signal; high leakage risk |
| Oil imputation and lags | Medium | Test validation impact |
| Fourier terms | Medium | Smooth seasonality |
| Interaction features | Medium | Can improve grouped demand patterns |
| Transactions | Low | EDA first; model only if future-safe |

