# Phase Checklist

## Phase 0 - Setup and Project Structure

- [x] Create repository structure
- [x] Draft README
- [x] Draft case study
- [x] Draft planning document
- [x] Draft architecture document
- [x] Draft data dictionary
- [x] Draft leakage checklist
- [x] Draft validation strategy
- [x] Draft deployment plan
- [ ] Download Kaggle data into `data/raw/`
- [ ] Confirm competition metric

## Phase 1 - Data Integration and EDA

- [ ] Load all CSV files
- [ ] Check shapes, columns, dtypes, and missing values
- [ ] Convert dates
- [ ] Confirm train and test date ranges
- [ ] Check key uniqueness
- [ ] Inspect store metadata
- [ ] Inspect oil gaps
- [ ] Inspect holiday collisions
- [ ] Inspect transactions
- [ ] Create EDA plots
- [ ] Write EDA observations

## Phase 2 - Feature Engineering

- [ ] Create calendar features
- [ ] Add store metadata
- [ ] Add holiday features
- [ ] Add oil features
- [ ] Add lag features
- [ ] Add rolling features
- [ ] Add Fourier features
- [ ] Validate leakage rules
- [ ] Confirm train/test feature consistency

## Phase 3 - Model Selection and Validation

- [ ] Define holdout validation period
- [ ] Implement RMSLE
- [ ] Train naive baseline
- [ ] Train deterministic baseline
- [ ] Train first LightGBM model
- [ ] Train XGBoost or CatBoost comparison
- [ ] Tune best model
- [ ] Update experiment log

## Phase 4 - Error Analysis and Refinement

- [ ] Plot actual vs predicted
- [ ] Plot residuals over time
- [ ] Analyze errors by family
- [ ] Analyze errors by store
- [ ] Analyze holiday errors
- [ ] Analyze zero-sales behavior
- [ ] Plot feature importance
- [ ] Run refinement experiments

## Phase 5 - Submission and Portfolio Documentation

- [ ] Generate test features
- [ ] Confirm feature columns
- [ ] Generate predictions
- [ ] Clip predictions at zero
- [ ] Create `submission.csv`
- [ ] Submit to Kaggle
- [ ] Record public leaderboard score
- [ ] Polish README
- [ ] Finalize case study
- [ ] Finalize lessons learned

## Bonus Phase - Attention or Transformer Experiment

- [ ] Research candidate architecture
- [ ] Prepare grouped time-series format
- [ ] Define static features
- [ ] Define known future features
- [ ] Define observed past features
- [ ] Train small prototype
- [ ] Compare against best boosted-tree model
- [ ] Write complexity tradeoff

