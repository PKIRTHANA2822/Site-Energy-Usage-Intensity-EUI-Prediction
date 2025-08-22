# Project Title
- Site Energy Usage Intensity (EUI) Prediction — WiDS 2022 Datathon
<img width="900" height="550" alt="header_eui-1" src="https://github.com/user-attachments/assets/e952c959-eb6f-44e4-83a1-61e40b8f60df" />
# Objective
- Predict a building’s Site Energy Usage Intensity (EUI) using building metadata and climate variables so facility managers and city planners can make data‑driven decisions to reduce consumption and emissions.
# Why This Project
- Real‑world impact: Buildings account for a significant portion of global energy use and CO₂ emissions. Accurate EUI forecasts inform retrofits, operations, and policy.
- Business value: Prioritizes investments by identifying high‑impact levers (insulation, HVAC, glazing).
- Technical breadth: Blends tabular ML, feature engineering, and interpretability—ideal to showcase end‑to‑end DS skills.
# Step‑by‑Step Approach (Blueprint)
- Problem framing
- Task: Supervised regression (target: site_eui).
- Success metrics: RMSE (primary), MAE and R² (secondary).
- Constraints: data leakage control, robust CV, reproducibility.
- Data access & versioning
- Source: WiDS 2022 Datathon building dataset (train/test + climate).
- Track versions with Git + DVC (or MLflow artifacts).
- Data understanding & EDA
- Schema validation, missingness, outliers, distributions, correlations.
- Target analysis: skew, heteroscedasticity, site/city differences.
- Data cleaning
- Impute missing numeric/categorical features.
- Standardize units if needed (areas, temperatures).
- Remove/repair impossible values; cap extreme outliers when justified.
# Feature engineering
- Date/seasonal, climate aggregations, intensity ratios, interactions.
- Encodings for categoricals; scaling for continuous variables.
# Feature selection
- Filter (correlation, mutual information), wrapper (RFE), and model‑based importance (GBDT, Lasso).
- Modeling
- Baselines: Linear/Ridge/Lasso.
- Non‑linear: RandomForest, XGBoost/LightGBM/CatBoost.
- Pipelines with ColumnTransformer; hyperparameter search (Optuna/RandomizedSearchCV).
# Evaluation
- Robust CV (e.g., GroupKFold by site_id/year_built bucket if leakage risk).
- Hold‑out test; error analysis; calibration plots; SHAP.
- Packaging & delivery
- Save model.pkl, preprocessing pipeline, inference.py.
- Produce predictions CSV for competition/test and a short model card.
# Exploratory Data Analysis (what to do)
- Data health
- Missingness map (per column, per row). Distinguish MCAR/MAR/MNAR.
- Cardinality & unique counts for all categoricals (e.g., building_class, facility_type, primary_use).
- Target (site_eui)
- Distribution & skew; log‑transform check; per‑site/region faceting.
- Stability: compare central tendency across climate zones.
# Numerical features
- Histograms, KDE, and boxplots by climate zone/site.
- Pairwise correlations; VIF for multicollinearity.
- Categoricals
- Average EUI by category; ANOVA/kruskal to see signal.
- Climate
- Degree days (HDD/CDD), monthly vs annual aggregates.
- Interaction ideas: HDD × heating_type.
- Outliers & quality
- Influence diagnostics; winsorization policy; domain‑driven rules (e.g., floor_area > 0).
- Quick EDA checklist: schema check → missingness → distributions → target behavior → correlations → climate joins → leakage sweep.
# Feature Selection (practical path)
- Filter level
- Drop near‑constant & >95% missing features.
- Remove one of any highly correlated pair (|r| > 0.95) unless domain‑critical.
- Embedded/model‑based
- L1 (Lasso) for sparse linear signal.
- Tree‑based importance (Permutation/SHAP) for non‑linear signal.
- Wrapper
- RFE with ElasticNet or LightGBM; stop at elbow on CV‑RMSE curve.
- Leakage guard
- Ensure engineered features use train‑only stats inside CV folds.
# Feature Engineering (ideas that work well on EUI)
- Target transformation: Evaluate log1p(site_eui) if heavy right‑tail; invert at inference.
- Climate aggregates: annual/monthly HDD & CDD, mean/variance of temp, humidity, wind.
- Intensity ratios: EUI_per_floor_area, window_to_wall, occupancy_density (if available).
- Age & vintage: current_year - year_built, binned vintage; renovated_flag.
- Operational: is_weekend, season, peak_hours_share proxies.
- Encodings: One‑hot for low‑cardinality; target/WOE or CatBoost encoding for high‑cardinality with CV.
- Interactions: climate × building type, area × height, HDD × heating fuel.
- Scaling: Standardize continuous features for linear models; optional for trees.
# Model Training (reference pipeline)
- Cross‑validation: 5‑fold GroupKFold by site_id (if available) to avoid information bleeding across folds.
- Baselines: DummyRegressor (median) → Ridge/Lasso → ElasticNet.
- Tree/Boosting: RandomForest, LightGBM/XGBoost/CatBoost with early stopping.
- Hyperparameter tuning: Optuna or RandomizedSearchCV; optimize CV‑RMSE.
- Reproducibility: set random_state, fix seeds, log with MLflow.
# Output & Deliverables
<img width="1320" height="750" alt="Screenshot 2025-08-22 145250" src="https://github.com/user-attachments/assets/511d1c2b-3d26-463d-a0ba-28cd04c3fdd4" />
<img width="1328" height="290" alt="Screenshot 2025-08-22 145302" src="https://github.com/user-attachments/assets/e5c92173-11b5-45d0-a1a7-31493c8b1c25" />

