# Corn Yields & Sustainable Growing Practices

**Do soil-health practices like no-till, cover crops, and crop rotation meaningfully hurt corn yields?**
An R analysis of 13 years of controlled field-trial data from Iowa State University.

**Author:** Evan Kennedy  |  **Tools:** R, tidyverse, modelr, R Markdown

---

## Problem

Farmers are slow to adopt sustainable practices (no-till, crop rotation, cover crops) largely because they fear yield losses. This project asks how much that fear is justified: when growing practices are used to model corn yield, how much do they actually matter?

**Hypothesis:** Sustainability-focused practices have little effect on corn yields.

## Data

Farm-level yield data paired with practices is rarely public, so this project uses data from a controlled field study, which is the next best option.

- **Source:** *Thirteen-Year Stover Harvest and Tillage Effects on Corn Agroecosystem Sustainability in Iowa* (USDA National Laboratory for Agriculture and the Environment / Iowa State University), available on [Data.gov](https://catalog.data.gov/dataset/thirteen-year-stover-harvest-and-tillage-effects-on-corn-agroecosystem-sustainability-in-i-be5ae)
- **Files used:**
  - `Field 70-71 CornYield_2008-2021_ForR_0.csv`: plot treatments and yields
  - `PRISM_MayTemps.csv`: May temperatures (PRISM Group, Oregon State University)
  - `Rainfall_AEA.csv`: daily rainfall
- **Analysis unit:** one plot in one year (bushels/acre), covering 2008–2020 after joining weather data
- **Practices studied:** tillage (chisel vs. no-till), cropping system (continuous, twin-row, cover), crop rotation (continuous corn, corn-alfalfa, cover crop), and stover harvest level

## Method

1. **Cleaning:** simplified messy treatment labels with string matching, handled missing values, and reshaped wide per-year yield columns into one row per plot-year (`pivot_longer`).
2. **Feature engineering:** summed daily April–October rainfall into seasonal totals and joined them with May temperature by year.
3. **Exploratory analysis:** violin plots and group means for every practice and weather variable.
4. **Modeling:** multiple linear regression with dummy-coded practices, using conventional practices as the baseline. Two models were compared:
   - **Full model:** practices + seasonal rainfall + May mean temperature
   - **Practices-only model:** practices alone
5. **Validation:** leave-one-year-out cross-validation. Each year was held out in turn so the model never saw the weather or conditions of the year it predicted. Metrics: RMSE and R².

## Results

- **Practices barely move average yields.** Differences were small, and the conventional option was slightly ahead each time:

  | Practice | Conventional | Sustainable |
  |---|---|---|
  | Tillage | Chisel: 186 bu/ac | No-till: 182 bu/ac |
  | Growing system | Continuous: 186 bu/ac | Cover: 184, Twin: 181 bu/ac |
  | Rotation | Continuous corn: 186 bu/ac | Cover crop: 183, Corn-alfalfa: 180 bu/ac |

- **The models have essentially no predictive power.** Average RMSE was ~29 bu/ac (full) and ~25 bu/ac (practices-only), with mostly negative R², meaning they did no better than guessing the mean.
- **Interpretation:** practices explain little of the variation in yield. Year-to-year conditions (weather, pests, etc.) matter far more, so adopting these practices poses a small yield risk relative to other factors.

> **Takeaway:** no-till, rotation, and intercropping showed no strong negative impact on corn yields in this dataset.

## Reproducibility

- Full analysis lives in a single R Markdown file: `Corn_Yields_and_Sustainable_Growing_Practices.Rmd`, which knits to HTML or PDF.
- Only two packages are needed: `tidyverse` and `modelr`.
- Evaluation uses deterministic leave-one-year-out splits, so no random seed is needed and results are identical on every run.
- Every cleaning, join, and modeling step is in the document with explanatory prose.

**To run:**
1. Download the three CSVs from the Data.gov link above.
2. Place them in the same folder as the `.Rmd`. Files are read via relative paths.
3. Run `install.packages(c("tidyverse", "modelr"))`.
4. Knit the `.Rmd` in RStudio.

## Limitations

- **Narrow scope:** one research site in Iowa, one managing team, and a single crop. Results may not transfer to other climates, soils, or farm management styles.
- **Weak weather inputs:** May temperature and seasonal rainfall are crude proxies. Rainfall timing, heat stress, pests, and fertilizer rates were not available, and these likely drive much of the unexplained variance.
- **Few distinct weather values:** with only 13 years, weather variables take just 13 distinct values, which limits what the models can learn.
- **Linear model:** a poor fit does not rule out nonlinear or interaction effects between practices.
- **Not proof of "no effect":** low predictive power shows that practices are not a major driver of yield. It does not prove they have zero effect.
- **Intended use:** a screening-level answer to "should I worry about yield loss?", not a forecasting tool or a causal estimate for any specific farm.
