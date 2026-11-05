# Building a Management Aid for Small Farms

A guided, data-driven tutorial on building a simple corn yield prediction tool that small farms can use to evaluate growing plans, without paying for precision-agriculture consulting.

**Author:** Evan Kennedy

---

## Overview

Precision agriculture is a boon for large industrial farms, which can easily recoup the cost of expert data advisement. Smaller farms often can't. This project walks through a simple, low-cost pipeline that a grower could follow using their own historical field records:

1. **Data processing:** import messy Excel records, clean them, and merge site-level and plot-level tables into one DataFrame.
2. **Exploratory analysis:** visualize how fertilizer, irrigation, and temperature relate to yield and nutrient uptake.
3. **Modeling:** fit a regularized linear regression (via `SGDRegressor`) to predict corn yield, using cross-validation to choose polynomial degree.
4. **Application:** use the trained model to evaluate a hypothetical growing plan.
5. **Interpretation:** read the model coefficients to see which inputs matter most.

### Key results

- Cross-validation showed that degree 1 (plain linear) generalizes best; higher polynomial degrees overfit.
- On the held-out test set (70/30 split), the model achieves roughly **±19 bu/acre RMSE** and **R² ≈ 0.70**.
- Irrigation and nitrogen have the clearest positive effects on yield. Phosphorus, potassium, and sulfur matter less than the levels already present in the soil, consistent with them acting as supplements for soil deficits rather than general yield boosters.

---

## Repository Contents

```
.
├── Building_a_Management_Aid_for_Small_Farms.ipynb   # The full tutorial notebook
├── README.md
└── Nebr_HiYieldCornNPKS_Feb_2021.xls                 # NOT INCLUDED, see "Getting the Data" below
```

---

## Getting the Data (Required)

**The dataset is not included in this repository** because the file is too large to host here. You need to download it yourself before running the notebook.

### Source

The data comes from a University of Nebraska-Lincoln Agricultural Research Division study on experimentally controlled corn plots, collected 2002–2004.

- **Dataset (Dryad):** https://datadryad.org/dataset/doi:10.5061/dryad.p30c6
- **Original publication:** https://acsess.onlinelibrary.wiley.com/doi/10.2134/agronj2008.0103x

### Steps

1. Go to the Dryad link above and download the dataset files.
2. Locate the Excel file **`Nebr_HiYieldCornNPKS_Feb_2021.xls`**.
3. Place it **in the same folder as the notebook**.
4. Confirm the filename matches the `data_file` variable in the notebook:

   ```python
   data_file = "Nebr_HiYieldCornNPKS_Feb_2021.xls"
   ```

   If your downloaded file has a different name, either rename the file or update this variable.

### Expected structure

The notebook expects the workbook to contain two sheets:

| Sheet name | Contents |
|---|---|
| `Site-year information` | Per site-year data (soil properties, hybrid, GDD, irrigation, etc.) |
| `Data` | Per plot data (treatments, fertilizer rates, yields, nutrient uptake) |

If the sheet names in your copy differ, update the `sheets['...']` lookups in the import cell.

### Using your own data

The pipeline is designed to generalize. To use your own field records, put them in an Excel file with the same two-table layout (a site/season table and a per-plot or per-treatment table joined by a shared identifier). You will likely need to adjust:

- the file name and sheet names,
- the lists of columns to keep (`data_features_to_keep`, `site_features_to_keep`, etc.),
- the join key (`Site-yr`),
- the missing-value handling for your data.

Historical data from your own fields will generally produce a more useful model than the Nebraska sample data.

---

## Setup

### Requirements

- Python 3.9+
- Jupyter Notebook or JupyterLab
- An editor or IDE of your choice (e.g., VS Code)

### Python packages

```bash
pip install xlrd numpy pandas matplotlib seaborn scikit-learn
```

Notes:
- `xlrd` is needed to read the older `.xls` Excel format.
- `seaborn` is imported for the violin plots. The notebook's install cell doesn't list it, so install it too.
- `root_mean_squared_error` requires a recent scikit-learn (1.4+).

### Running

```bash
jupyter notebook Building_a_Management_Aid_for_Small_Farms.ipynb
```

Then run the cells in order. Make sure the data file is in place first (see above).

---

## Notebook Walkthrough

| Section | What happens |
|---|---|
| **Data Processing** | Loads both Excel sheets; fixes the site table's header and drops empty columns; trims to relevant features; merges tables on `Site-yr`; drops observations missing `Hybrid` and imputes missing `GDDsilk` with the same-year average. |
| **Exploratory Analysis** | Scatter and violin plots of yield vs. N/P/K/S application, irrigation, and growing degree days, plus nutrient uptake vs. application. |
| **Modeling** | One-hot encodes categorical features, z-score normalizes continuous ones, splits 70/30, picks polynomial degree via 5-fold cross-validation, and fits `SGDRegressor`. |
| **Using the Model** | Walks through preprocessing a new growing plan (normalizing with training-set statistics, building dummy columns) and predicting its yield. |
| **Interpretation** | Examines model coefficients to compare the influence of each input. |

---

## Limitations

- The model is trained on experimental plots in Nebraska from 2002–2004. Predictions for very different climates, soils, or hybrids should be treated cautiously.
- Yield is also driven by unpredictable events (drought, heat waves, cold shocks, pest outbreaks) that are not in the model.
- A linear model is simple and cheap to train, but more complex methods can often predict better given more data (e.g., topographical data).
- Some features were dropped because they weren't recorded for every treatment group. The notebook includes a commented-out list of additional outputs (e.g., dry matter, kernel counts) that could be modeled with more complete data.

---

## Data Citation & Attribution

Data originally collected by the University of Nebraska-Lincoln Agricultural Research Division and published via Dryad (https://datadryad.org/dataset/doi:10.5061/dryad.p30c6). Please consult the Dryad page for the dataset's license and preferred citation, and cite the original study when reusing the data.
