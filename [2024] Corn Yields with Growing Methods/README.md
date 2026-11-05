# Corn Yields and Sustainable Growing Methods
A data-driven analysis of the impact sustainable growing practices have on corn yields, structured within the notebook as a tutorial both for readability and reproducibility.

**Author:** Evan Kennedy

---

## Project Overview

This project investigates the relationship between sustainable agricultural practices and corn yields using historical data from a controlled agricultural field study conducted in Iowa between 2008 and 2020.

As sustainable farming practices become increasingly important for preserving soil health and reducing environmental damage, concerns about their potential impact on crop productivity remain a barrier to adoption. This project uses exploratory data analysis and polynomial regression to investigate whether practices such as no-till farming, crop rotation, and reduced stover harvesting have a meaningful effect on corn yields.

**Central Research Question:** To what extent do sustainable growing practices influence corn yields, and do these practices impose a measurable yield cost on farmers?

**Hypothesis:** Growing practices related to sustainability have little effect on corn yields.

## Technologies Used

- **Python** – Primary programming language
- **Pandas** – Data cleaning, processing, and manipulation
- **NumPy** – Numerical operations and feature engineering
- **Matplotlib & Seaborn** – Data visualization and exploratory analysis
- **Scikit-learn** – Polynomial regression, model training, and cross-validation

## Data Sources

The project uses data from a National Laboratory for Agriculture and Environment study conducted through Iowa State University, examining the effects of different agricultural treatments on corn yields and soil sustainability.

[Dataset: Thirteen-Year Stover Harvest and Tillage Effects on Corn Agroecosystem Sustainability in Iowa](https://catalog.data.gov/dataset/thirteen-year-stover-harvest-and-tillage-effects-on-corn-agroecosystem-sustainability-in-i-be5ae)

Three datasets were incorporated:

- **Field Treatment and Yield Data:** Corn yields and agricultural management practices for individual experimental plots.
- **Temperature Data:** Annual May temperature measurements, including maximum, minimum, and mean temperatures.
- **Rainfall Data:** Daily rainfall measurements, aggregated into seasonal totals.

## Methodology

### 1. Data Collection and Processing

The three datasets were imported into Pandas DataFrames and processed into a unified dataset, with each observation representing an individual plot in a particular growing year.

Key processing steps included:

- Aggregating daily rainfall measurements from April through October into annual seasonal totals.
- Extracting biochar application levels from existing treatment descriptions.
- Standardizing agricultural system and crop rotation classifications.
- Separating individual plot yields by year.
- Combining yield, agricultural treatment, temperature, and rainfall information into a single dataset.

The resulting dataset included agricultural management variables such as tillage method, crop rotation, growing system, stover harvest level, and biochar application, alongside environmental variables such as seasonal rainfall and temperature.

### 2. Exploratory Data Analysis

Exploratory analysis was conducted to investigate relationships between corn yields and individual agricultural and environmental variables.

Violin plots were used to examine yield distributions across:

- **Environmental factors:** Annual rainfall, mean temperature, maximum temperature, and minimum temperature.
- **Agricultural practices:** Tillage method, crop rotation, growing system, and stover harvest level.
- **Other variables:** Biochar application and growing year.

This analysis provided an initial assessment of which variables appeared to influence yield distributions and informed subsequent feature selection.

### 3. Feature Engineering and Model Preparation

Categorical agricultural variables were converted into numerical representations suitable for regression modeling.

The dataset was shuffled and divided into approximately 70% training data and 30% testing data.

Features were selected based on the exploratory analysis, with particular attention given to retaining agricultural management variables relevant to the project's central research question.

### 4. Polynomial Regression and Model Selection

A polynomial regression model was developed using Scikit-learn to investigate the combined relationship between agricultural practices, environmental conditions, and corn yields.

Model degrees ranging from 1 through 10 were evaluated using 5-fold cross-validation.

Mean validation losses were compared to identify an appropriate model complexity while balancing predictive performance and interpretability.

A sixth-degree polynomial regression model was ultimately selected because its validation loss was comparable to higher-degree alternatives while offering a simpler model.

### 5. Model Evaluation

The selected model was evaluated using held-out test data.

Evaluation included:

- **Mean Squared Error (MSE):** Measuring the magnitude of prediction errors.
- **R² Score:** Assessing how well the model captured variation in actual corn yields.
- **Prediction Visualization:** Comparing the distributions of predicted and actual yields across growing years using violin plots.

## Findings and Conclusions

Exploratory analysis revealed relatively little variation in corn yield distributions across the different agricultural practices examined.

The regression model also struggled to predict yields accurately, producing a negative R² score and relatively high prediction error. Its results suggested that the agricultural management variables included in the analysis had limited predictive value for corn yields within the dataset.

The project's primary findings were:

1. **Limited yield differences between growing practices:** No-till farming, crop rotations, growing systems, and reduced stover harvesting showed relatively little relationship with observed corn yields.
2. **Environmental conditions matter:** Rainfall and temperature appeared more relevant to yield variation than the agricultural management variables examined.
3. **Agricultural practices alone are insufficient predictors:** More detailed information about growing conditions, including fertilizer use, soil characteristics, irrigation, and pest activity, would likely be necessary for more accurate yield predictions.

The findings support the initial hypothesis that the sustainable agricultural practices examined have relatively little effect on corn yields.

Importantly, this suggests that adopting practices intended to improve long-term soil sustainability does not necessarily require sacrificing short-term crop productivity.

## Limitations and Future Improvements

Several limitations should be considered when interpreting the results:

- **Dataset scope:** The analysis relies on controlled experimental plots from a single agricultural research setting rather than a representative sample of commercial farms.
- **Limited environmental information:** Detailed information about fertilizer application, soil conditions, irrigation, pests, and other potentially influential factors was unavailable.
- **Predictive limitations:** The regression model's negative R² score indicates that the selected features and modeling approach were insufficient for accurately predicting yields.
- **Generalizability:** Results from this particular study may not apply equally across different geographic regions, soil conditions, climates, or agricultural systems.

Future work could incorporate additional environmental and agricultural datasets, investigate alternative modeling approaches, and examine whether the observed relationships hold across a broader range of growing conditions.

## Repository Structure

The project notebook contains the complete workflow, including data collection, preprocessing, exploratory analysis, feature engineering, regression modeling, and evaluation.

The three original CSV datasets are required to execute the notebook.
