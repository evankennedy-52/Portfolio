# Building a Management Aid for Small Farms

**A step-by-step Python tutorial that turns a farm's routine records into a low-cost corn yield estimator.**
Built as a Jupyter notebook, using Nebraska field-trial data.

**Author:** Evan Kennedy  |  **Tools:** Python, pandas, NumPy, scikit-learn, matplotlib, seaborn

---

## Problem

Precision agriculture helps large farms boost efficiency, but the consulting behind it is often too expensive for small family farms. This project shows that a small operation can build its own yield-estimation tool in a few hours, on a basic computer, using data it likely already keeps in spreadsheets.

The notebook is written as a tutorial for growers and non-specialists. It explains each step, including why the step is done, and links to further reading.

## Data

- **Source:** University of Nebraska-Lincoln high-yield corn experiment (2002–2004), via [Dryad](https://datadryad.org/dataset/doi:10.5061/dryad.p30c6) (original paper: [Agronomy Journal](https://acsess.onlinelibrary.wiley.com/doi/10.2134/agronj2008.0103x)). File used: `Nebr_HiYieldCornNPKS_Feb_2021.xls`.
- **Structure:** two Excel tables, one of site-level info (soil, pH, climate, hybrid) and one of plot-level treatments and outcomes, joined into one table of ~1,400 plot observations.
- **Inputs (23 raw features):** fertilizer rates (N, P, K, S), irrigation, hybrid, soil properties, previous crop, tillage, planting population, seed weight, and growing-degree-day temperature measures.
- **Target:** grain yield (bushels/acre).
- **Built for:** any farm that keeps historical field records in a similar format. Swapping in your own field's data makes the output more useful than the sample data.

## Method

1. **Import & clean:** loaded multi-sheet Excel files into pandas, fixed a mis-read header row, and dropped empty columns.
2. **Feature trimming:** removed features that were redundant or not recorded for every treatment group.
3. **Merge:** joined site and plot tables on a shared site-year ID.
4. **Missing values:** dropped 40 rows missing `Hybrid`; imputed missing silking GDD (~10% of rows) with the same-year average across nearby sites.
5. **Exploratory analysis:** scatter and violin plots of yield and nutrient uptake against fertilizer, irrigation, and temperature.
6. **Preprocessing:** 70/30 train–test split, z-score scaling of continuous features, and one-hot encoding of categoricals (65 model inputs).
7. **Modeling:** L2-regularized linear regression (`SGDRegressor`), with 5-fold cross-validation to choose polynomial degree (1–3).
8. **Application:** a worked example that runs a candidate growing plan through the model to estimate yield.

## Results

- **Degree 1 (plain linear) won clearly.** Cross-validated RMSE was ~19 bu/ac, while degrees 2 and 3 overfit badly and produced errors in the billions.
- **Test-set performance:** RMSE ≈ **19.4 bu/ac** and R² ≈ **0.74**, so the model explains roughly 70% of yield variance using only information available before the season.
- **Findings from the coefficients:** nitrogen and irrigation were the clearest controllable yield drivers. P, K, and S inputs mattered less than the nutrient levels already in the soil, consistent with their role as a fix for deficiencies rather than a general yield boost.
- **Demo:** a sample plan predicted 223 bu/ac against an actual 205 bu/ac, which is useful for sanity-checking a plan, not for exact forecasting.

## Reproducibility

- Everything lives in one notebook: `Building_a_Management_Aid_for_Small_Farms.ipynb`.
- Random seeds are fixed (`random_state=25`) for the data split, model, and sample row.
- Scaling for new inputs reuses the training-data mean and standard deviation, so plans run through the model are processed the same way as the training data.

**To run:**
1. Install Python and Jupyter, then `pip install xlrd numpy pandas matplotlib scikit-learn seaborn` (the notebook has these as a ready-to-uncomment cell).
2. Download the "Sep 06, 2022" files from the Dryad link above and place them in the same folder as the notebook.
3. Confirm the `data_file` variable matches the downloaded filename, then **Run All**.

To use your own farm's data, save it in the same table layout and update the file name and feature lists.

## Limitations

- **Limited scope:** three years of experimental plots from Nebraska research sites, with high-yield, largely irrigated management. Results may not carry over to other regions, soils, or typical farm conditions without retraining on local data.
- **Not weather-proof:** drought, heat waves, and pests aren't available at planning time and aren't modeled. Temperature enters as projected GDD.
- **Associations, not causes:** coefficients describe patterns in this dataset (and several, such as hybrid and site, overlap), so they shouldn't be read as guaranteed effects of changing a single input.
- **Intended use:** a lightweight check that a growing plan lands in a reasonable yield range, meant to complement a grower's experience rather than replace it.
