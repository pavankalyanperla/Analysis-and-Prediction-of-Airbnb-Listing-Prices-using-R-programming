# Analysis and Prediction of Airbnb Listing Prices using R

A complete data science workflow in R — from raw data ingestion through exploratory analysis to predictive price modelling — applied to a Seattle Airbnb listings dataset (~Dec 2018). The project demonstrates data cleaning, geospatial visualisation, regression diagnostics, and ensemble modelling via a Jupyter notebook with an IRkernel backend.

---

## Dataset

| Property | Value |
|----------|-------|
| Source | Seattle Airbnb scrape (~December 2018) |
| File | `dataset/airbnb_seattle.csv` |
| Rows | 7,576 listings |
| Columns | 18 (see below) |
| Target variable | `price` (nightly rate in USD) |

**Columns:** row index, `room_id`, `host_id`, `room_type`, `address`, `reviews`, `overall_satisfaction`, `accommodates`, `bedrooms`, `bathrooms`, `price`, `last_modified`, `latitude`, `longitude`, `location`, `name`, `currency`, `rate_type`

**Missing values:** 1,473 NAs in `overall_satisfaction` (imputed with column mean); 2 NAs in `bathrooms` (imputed with 0).

---

## Repository Structure

```
├── dataset/
│   └── airbnb_seattle.csv            # Source data (uncompressed)
├── Script or code/
│   └── Analysis and Prediction of Airbnb Listing Prices.ipynb
├── Docs/
│   ├── Analysis and Prediction of Airbnb Listing Prices.docx
│   └── R Project Final Report.pdf
├── REPO_AUDIT.md                     # Pre-refactor audit report
├── .gitignore
└── README.md
```

---

## Setup

### R version

R 4.4.0 or later is required. The notebook was developed and executed on **R 4.4.0** with packages from CRAN (binary builds for Windows).

### Required packages

Install all dependencies once before opening the notebook:

```r
install.packages(c(
  "readr",        # CSV import
  "dplyr",        # Data manipulation
  "ggplot2",      # Visualisations
  "tidyr",        # NA handling
  "ggcorrplot",   # Correlation matrix plot
  "ggmap",        # Stadia Maps geographic overlay
  "treemapify",   # Treemap visualisation
  "caret",        # Train/test splitting
  "randomForest", # Random Forest model
  "rpart",        # Decision tree model
  "car",          # VIF / multicollinearity check
  "knitr",        # Notebook table formatting
  "gridExtra"     # Multi-panel plot layout
))
```

### Stadia Maps API key (optional — for geographic map cells only)

Two cells in the *Geographical Scatterplot* section download a base map from Stadia Maps. These cells are clearly labelled and will produce an error if no key is configured; all other cells run normally without a key.

To enable the map cells:
1. Register for a free API key at <https://stadiamaps.com/> (no credit card required for low-volume use).
2. Add the following line to your `.Renviron` file (run `usethis::edit_r_environ()` to open it):
   ```
   STADIA_MAPS_API_KEY=your_key_here
   ```
3. Restart your R session (or the Jupyter kernel).

### Running the notebook

Open `Script or code/Analysis and Prediction of Airbnb Listing Prices.ipynb` in Jupyter with the **IR (IRkernel)** kernel selected, then run all cells.

```bash
# Install IRkernel if not already registered with Jupyter
Rscript -e "install.packages('IRkernel'); IRkernel::installspec()"

# Launch Jupyter
jupyter notebook
```

---

## EDA Highlights (real numbers from notebook output)

- **Price distribution:** Median $88/night, mean $113/night, range $15–$5,900. 96.5% of listings are priced at or below $300; **70% fall in the $50–$150 band**.
- **Location:** 89.6% of listings are in Seattle proper (6,791 of 7,576). The next largest cities are Bellevue (322), Kirkland (202), and Redmond (110). Seattle's mean price is **$112/night**.
- **Room type:** Entire home/apartment commands the highest mean price (~$149/night), private rooms are significantly lower (~$65/night), and shared rooms are cheapest (~$37/night).
- **Physical capacity:** Both bedrooms and bathrooms show strong positive correlation with price. Listings with 1 bathroom account for ~74% of all listings; 1-bedroom listings account for 56.5%.
- **Rating paradox:** Average ratings cluster tightly in 4.80–5.00; the narrow range makes this a weak price predictor.

---

## Modelling Results

### Train / Validation / Test Split

| Set | Rows | Purpose |
|-----|------|---------|
| Training | 5,452 | Model fitting |
| Validation | 1,365 | Model selection |
| **Test (held-out)** | **759** | **Final unbiased evaluation** |

The test set is created first and kept completely separate throughout model development. The validation RMSE is used only for model selection; the test RMSE is the reportable, unbiased performance estimate.

### Model Comparison

| Model | Validation RMSE | Validation R² | Notes |
|-------|-----------------|---------------|-------|
| Baseline Median | 181.97 | — | Naïve benchmark: predicts training median for every listing |
| Linear Regression | 164.87 | 0.159 | All 7 predictors significant; interpretable coefficients |
| Random Forest (ntree=500) | 153.04 | 0.276 | Captures non-linear interactions |
| **Decision Tree** | **150.70** | **0.298** | Lowest validation RMSE; narrowly best on this split |

> **Note on R²:** The RF model printout shows "43.4% Var explained" — this is an OOB estimate computed on training-data held-out bags, not on the held-out validation set. The linear model's `summary()` R² = 0.3354 is a training-fit (in-sample) metric. The two are not on the same basis and should not be compared directly. The validation R² column above gives the correct apples-to-apples comparison for all models.

**Final held-out test RMSE (Decision Tree): 70.28**

### Linear Model Summary

```
Multiple R-squared: 0.3354,   Adjusted R-squared: 0.3346
F-statistic: 392.5 on 7 and 5444 DF,  p-value: < 2.2e-16

Coefficients (all p < 0.01):
  bathrooms    +$39.13 / additional bathroom   (strongest per-unit effect)
  rating       +$19.45 / rating point
  bedrooms     +$23.20 / additional bedroom
  accommodates  +$8.64 / additional guest slot
  reviews_sum   -$0.13 / additional review  (negative: budget listings get more bookings)
  longitude    -$123.07 / degree east      (distance from waterfront)
  latitude      +$67.77 / degree north
```

### VIF (Variance Inflation Factors)

All VIFs < 3.4 — no multicollinearity concerns.

```
rating=1.02, reviews_sum=1.05, bedrooms=3.33, bathrooms=1.68,
accommodates=2.88, latitude=1.00, longitude=1.03
```

---

## Key Findings

1. **Physical capacity is the dominant pricing driver.** Bedrooms, bathrooms, and accommodation capacity are the top 3 features by importance in both the Random Forest (%IncMSE: 19.8, 14.1, 12.5) and the linear model (highest standardised coefficients). Adding a bathroom adds more per-night value (+$39) than adding a bedroom (+$23).

2. **Location matters less than expected.** Despite the geographic spread across Seattle and surrounding cities, lat/lon features rank 6th and 4th in the Random Forest. Over 89% of listings concentrate in Seattle, limiting spatial price variation.

3. **Rating is a weak predictor.** The near-universal 4.8–5.0 rating range provides almost no discriminative signal (RF rank: 7th, VIF near 1).

4. **More reviews → lower price (counterintuitive).** Budget listings get more bookings and therefore more reviews. The negative `reviews_sum` coefficient is real and interpretable, not an artifact.

5. **Linear model assumptions are partially violated.** Residual plots show heteroscedasticity (variance grows with fitted price) and right-tailed non-normality, driven by extreme-price outliers. A log-price target would likely improve assumption validity.

---

## Production Recommendation

For **price estimation accuracy**, use the **Random Forest**: on the held-out validation set it achieves R² = 0.276 vs R² = 0.159 for linear regression, and it is robust to the outliers and non-linearities that violate linear model assumptions. A single decision tree (the best model on this split) is less stable — small training-set changes can produce very different trees.

For **host-facing price explanation** ("why is my listing priced at X?"), use the **linear model**: its coefficients give direct, actionable guidance in dollars per unit change.

---

## Possible Next Steps

- **Encode `room_type` and `city`** as dummy variables and add to the model formula — `room_type` in particular shows a large mean price difference in EDA (~$84 gap between Entire home and Private room) but is excluded from the current model formula.
- **Log-transform the price** to address heteroscedasticity and improve linear model assumption validity (OLS performance on log-scale would be higher for balanced-error metrics).
- **Hyperparameter tuning for Random Forest** via `caret::train()` with cross-validation — optimise `mtry` (features per split) and experiment with `ntree`.
- **Geospatial feature engineering** — raw lat/lon is a weak location signal. Distance from Pike Place Market, neighbourhood-level price indices, or OpenStreetMap-derived walkability scores would add richer spatial signal.
- **Outlier handling** — listings priced >$500/night (~3.5% of data) likely account for a disproportionate fraction of RMSE. Capping the training distribution or fitting a separate high-price model could improve overall accuracy.
- **Time-based features** — the dataset is a snapshot from December 2018. A longitudinal dataset would allow seasonal and trend features.
