# Data Dictionary

This file documents the expected Kaggle input files, important columns, and planned modeling use. Update this after loading the actual data and confirming dtypes, missing values, and date ranges.

## File Overview

| File | Grain | Purpose | Modeling Use |
|---|---|---|---|
| `train.csv` | `date x store_nbr x family` | Historical supervised training rows | Main target and historical feature source |
| `test.csv` | `date x store_nbr x family` | Forecast horizon rows | Rows requiring predictions |
| `stores.csv` | `store_nbr` | Store metadata | Static store-level features |
| `oil.csv` | `date` | Daily oil price | Economic signal after imputation |
| `holidays_events.csv` | Event rows by date and locale | Holidays and events | Calendar shock features |
| `transactions.csv` | `date x store_nbr` | Store traffic signal | EDA first; model only if future-safe |
| `sample_submission.csv` | `id` | Submission template | Output schema |

## train.csv

| Column | Meaning | Expected Type | Modeling Use | Notes |
|---|---|---|---|---|
| `id` | Unique row identifier | Integer | Submission alignment only | Do not use as predictive signal |
| `date` | Sales date | Date | Calendar, split, joins | Parse as datetime |
| `store_nbr` | Store identifier | Integer/category | Store-level feature | Join to `stores.csv` |
| `family` | Product family | Category | Product feature | Key grouping for lags |
| `sales` | Unit sales target | Float | Target | Must be non-negative |
| `onpromotion` | Items on promotion | Integer | Known future feature if in test | Strong demand signal |

## test.csv

| Column | Meaning | Expected Type | Modeling Use | Notes |
|---|---|---|---|---|
| `id` | Unique row identifier | Integer | Submission alignment | Preserve row order |
| `date` | Forecast date | Date | Calendar, holiday, oil, lag context | Must use same feature pipeline |
| `store_nbr` | Store identifier | Integer/category | Store-level feature | Join to `stores.csv` |
| `family` | Product family | Category | Product feature | Group key |
| `onpromotion` | Items on promotion | Integer | Known future feature | Available in test |

## stores.csv

| Column | Meaning | Modeling Use |
|---|---|---|
| `store_nbr` | Store identifier | Join key |
| `city` | Store city | Categorical feature |
| `state` | Store state | Categorical feature |
| `type` | Store type | Categorical feature |
| `cluster` | Store cluster | Categorical or numeric feature |

## oil.csv

| Column | Meaning | Modeling Use |
|---|---|---|
| `date` | Oil price date | Join key |
| `dcoilwtico` | Daily WTI oil price | Economic feature after imputation |

Planned oil features:

- Imputed daily price
- Lagged oil price
- Rolling mean oil price
- Missingness indicator if useful

## holidays_events.csv

| Column | Meaning | Modeling Use |
|---|---|---|
| `date` | Holiday/event date | Join key |
| `type` | Holiday type | Feature or grouped flag |
| `locale` | National, regional, local | Feature or flag |
| `locale_name` | Country/state/city target | Feature or conditional join logic |
| `description` | Event description | Grouped event category |
| `transferred` | Whether holiday was transferred | Flag |

Holiday data must be aggregated before merging to the sales grain to avoid duplicating rows.

## transactions.csv

| Column | Meaning | Modeling Use |
|---|---|---|
| `date` | Transaction date | Join key |
| `store_nbr` | Store identifier | Join key |
| `transactions` | Store transaction count | EDA signal; modeling only if future-safe |

Decision rule:

- Use for EDA by default.
- Use as a feature only if the same signal would be available at prediction time or if a separate forecast of transactions is introduced.

## Data Quality Checks

- Confirm date ranges for every file.
- Confirm `id` uniqueness in train, test, and sample submission.
- Confirm uniqueness of `date x store_nbr x family` in train and test.
- Confirm no negative target values.
- Confirm holiday joins do not change row count.
- Confirm missing oil values before and after imputation.
- Confirm test feature generation has no unexpected missing columns.

