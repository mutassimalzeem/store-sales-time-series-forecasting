# EDA Plan

## Objective

Understand the structure, trends, seasonality, missing values, and external events in the data before modeling.

## Load and Inspect

- Load all CSV files.
- Check shapes, columns, dtypes, and missing values.
- Convert date columns to datetime.
- Inspect train and test date ranges.
- Confirm forecast horizon.
- Check uniqueness of `id`.
- Check uniqueness of `date x store_nbr x family`.

## Relational Mapping

- Merge sales rows with store metadata.
- Inspect oil price date coverage and missing values.
- Prepare holiday features before merging.
- Inspect transactions and decide if they are EDA-only.
- Create or update the data dictionary.

## Target Analysis

Planned visuals:

- Total sales over time
- Sales by year and month
- Sales by day of week
- Sales by store type
- Sales by cluster
- Top product families by total sales
- Zero-sales frequency by family and store-family pair

Questions:

- Is there a visible trend over time?
- Are weekends meaningfully different?
- Are month-end or payday periods important?
- Which families dominate total sales?
- Which rows are often zero?

## External Shocks and Holidays

- Locate earthquake-related events.
- Plot sales around the 2016 earthquake period.
- Compare sales before, during, and after major holidays.
- Separate national, regional, and local holidays.
- Inspect transferred holidays, bridge days, and work days.

## Missing Values

- Check missing oil price dates.
- Compare forward fill, backward fill, interpolation, or combined imputation.
- Document why the selected strategy is leakage-safe.
- Check missing values after all joins.

## EDA Deliverable

The EDA phase is complete when the project can clearly explain:

- Main sales patterns
- Data relationships
- Important missing-value decisions
- Holiday and event behavior
- Modeling implications

