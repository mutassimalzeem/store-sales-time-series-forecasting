# Leakage Prevention

Leakage prevention is the central quality standard for this project. A feature is valid only if it would be available at prediction time for the row being predicted.

## Core Rule

For any row dated `D`, features may use:

- Static metadata known before `D`
- Calendar and holiday facts known before or on `D`
- Promotion values provided in train/test rows
- Oil values only according to a documented availability assumption
- Sales history strictly before `D` for lag and rolling target features

Features must not use:

- Future sales values
- Validation target values while generating validation features
- Test target values
- Random splits that mix past and future dates
- Directly merged holiday rows that duplicate the target grain

## Lag Features

Planned lag features by `store_nbr` and `family`:

- Lag 1
- Lag 7
- Lag 14
- Lag 28
- Lag 30

Rules:

- Group by `store_nbr` and `family`.
- Sort by date before shifting.
- Use shifted target values only.
- Manually inspect several rows after feature creation.
- For test rows, use historical training sales and previously known values only.

## Rolling Features

Planned rolling windows:

- 7-day mean and standard deviation
- 14-day mean and standard deviation
- 30-day mean and standard deviation

Rules:

- Shift target before rolling aggregation.
- Rolling windows must not include the current row target.
- Validation rows cannot use validation-period target values unless simulating an autoregressive process with a clearly documented recursive strategy.

## Holiday Features

Holiday and calendar features are generally future-known, but joins can still introduce data problems.

Rules:

- Aggregate holiday rows to date-level or carefully defined locale-level features before merging.
- Confirm the merge does not increase row count.
- Separate national, regional, local, transferred, bridge, work day, and event flags where useful.
- Document how collisions are handled when multiple events occur on the same date.

## Oil Features

Oil values need a documented availability assumption.

Possible strategies:

- Use same-day oil values if they are assumed known by forecast generation time.
- Use lagged oil values for stricter production realism.
- Use forward fill, backward fill, interpolation, or combined imputation after documenting the choice.

Recommended default:

- Use imputed oil price for EDA.
- Prefer lagged oil values for production-minded modeling.
- Track whether oil features improve validation score.

## Transactions

Transactions are likely not available for future dates in a real forecasting setting.

Default decision:

- Use transactions for EDA.
- Do not use transactions as a model feature unless future availability is justified.

## Validation Checklist

Review before every modeling run:

- [ ] Validation data comes after training data
- [ ] Random shuffle is not used
- [ ] Lag features use only past target values
- [ ] Rolling features are shifted before aggregation
- [ ] Test features do not use test target values
- [ ] Holiday and calendar features are known in advance
- [ ] Oil treatment is documented
- [ ] Transactions are excluded unless future-safe
- [ ] Feature engineering logic is shared between train and test
- [ ] Predictions are clipped to non-negative values before RMSLE and submission

