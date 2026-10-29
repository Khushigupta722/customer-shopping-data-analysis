# Customer Shopping Data Analysis

A Python notebook for cleaning customer shopping data, exploring purchasing patterns, and exporting processed datasets. The project combines data quality checks, preprocessing, visual analysis, and automatically generated descriptive insights in a Google Colab workflow.

## Project Objective

The goal is to turn a customer shopping CSV into a cleaner dataset and explore questions such as:

- Which product categories appear most frequently?
- How does average purchase amount vary across categories and genders?
- Is there a relationship between age and purchase amount?
- Which numerical variables have the strongest correlations?
- What proportion of records are marked as subscribed?

This project performs exploratory data analysis (EDA). It does not train or evaluate a predictive machine learning model.

## Tools and Libraries

| Tool or library | Purpose |
| --- | --- |
| Python | Programming language |
| pandas | Data loading, cleaning, grouping, and CSV export |
| NumPy | Numerical operations |
| Matplotlib | Plot creation and formatting |
| Seaborn | Statistical visualizations |
| scikit-learn | Min–Max scaling and standardization |
| IPython | Displaying tables in the notebook |
| Google Colab | Notebook execution, CSV upload, and file download |

## Project Files

| File | Description |
| --- | --- |
| `customer_shopping_analysis.ipynb` | Main analysis notebook |
| `README.md` | Project documentation and execution instructions |

The input CSV is uploaded at runtime. Generated CSV files are saved in the Colab runtime and are not automatically added to the GitHub repository.

## Dataset Requirements

Upload exactly **one CSV file** when prompted. The notebook requires at least **1,000 rows and 10 columns**, reflecting the original assignment requirement.

Column names are converted to lowercase with underscores, and duplicate names are made unique. For example, `Purchase Amount (USD)` becomes `purchase_amount_usd`.

The following columns enable the shopping-specific analyses:

| Standardized column | Used for |
| --- | --- |
| `customer_id` or a numeric column ending in `_id` | Exclusion from numeric outlier treatment and scaling |
| `age` | Age-versus-purchase scatter plot |
| A column containing `purchase_amount` | Purchase amount comparisons |
| `category` | Category distributions and spending comparisons |
| `gender` | Spending summaries and grouped charts |
| A column containing `subscription_status` | Share of records with a value of `yes`, ignoring case and surrounding spaces |

Shopping-specific charts are skipped when their required columns are absent. General data quality checks and applicable distribution plots still run.

**Dataset attribution:** The dataset is supplied by the person running the notebook. Its original source and license are not specified here. Add the correct source link and sharing terms before distributing a dataset with this repository.

## Analysis Workflow

### 1. Load and inspect the data

- Upload a CSV and validate its dimensions.
- Inspect column names, data types, initial records, and descriptive statistics.
- Record the original row and column counts.

### 2. Clean column names and duplicate rows

- Standardize column names and resolve naming collisions.
- Identify and remove identical rows.
- Reset the index after duplicate removal.

### 3. Handle missing values

- Report missing-value counts and percentages.
- Fill numeric missing values with the column median.
- Fill categorical missing values with the most frequent value.
- Use `Unknown` for categorical columns with no observed values.
- Fill entirely missing numeric columns with zero and print a warning for review.

### 4. Detect and cap outliers

For each selected numeric feature, calculate the interquartile range:

`IQR = Q3 - Q1`

The capping bounds are:

- Lower bound: `Q1 - 1.5 × IQR`
- Upper bound: `Q3 + 1.5 × IQR`

Values outside these bounds are clipped to the corresponding bound. Numeric identifier columns named `customer_id` or ending in `_id` are excluded. The notebook displays boxplots before and after treatment.

### 5. Create scaled datasets

Two separate copies are created from the cleaned data:

- **Min–Max normalization:** Rescales selected numeric features to the range from 0 to 1 for this dataset.
- **Standardization:** Centers selected numeric features at zero and scales nonconstant features to unit population standard deviation.

The cleaned dataset remains a separate output. Numeric identifier columns are excluded from both transformations.

### 6. Perform exploratory analysis

The notebook includes:

- Missing-value bar chart.
- Numerical histograms and boxplots.
- Categorical count plots for columns with at most 30 unique values.
- Age-versus-purchase scatter plot.
- Purchase amount boxplots by category.
- Average purchase amount by gender.
- Pearson correlation heatmap.
- Pair plot using up to five varying numeric features and a reproducible sample of up to 1,000 rows.
- Average purchase amount by category and gender.
- Ranked absolute numerical correlations.

### 7. Generate insights and export results

The notebook produces up to five descriptive insights from the processed data, depending on the available columns, followed by a final data quality report and three CSV exports.

## How to Run in Google Colab

1. Download `customer_shopping_analysis.ipynb` from this repository.
2. Open [Google Colab](https://colab.research.google.com/).
3. Open the downloaded notebook using Colab's notebook upload option.
4. Run the cells in order, starting from the first cell.
5. At the upload prompt, select exactly one customer shopping CSV.
6. Review the charts, summaries, and generated insights.
7. Run the final cell to save the three processed datasets.

The cleaned CSV is downloaded automatically. Download the normalized and standardized CSVs from Colab's Files panel if needed.

The notebook uses `google.colab.files` for upload and download. Running it in another notebook environment requires replacing these operations with local file loading and saving.

### Dependencies

The libraries are normally available in Google Colab. If they are missing, run this in a new Colab code cell before the imports:

```python
%pip install pandas numpy matplotlib "seaborn>=0.12" scikit-learn ipython
```

## Output Files

| Output | Contents |
| --- | --- |
| `customer_shopping_cleaned.csv` | Data after duplicate removal, missing-value treatment, and outlier capping |
| `customer_shopping_minmax_scaled.csv` | Cleaned data with selected numeric features normalized |
| `customer_shopping_standardized.csv` | Cleaned data with selected numeric features standardized |

Charts and tables appear inside the notebook. They are not automatically saved as separate image files.

## Results and Validation

All 25 code cells were executed successfully with synthetic shopping data during preparation. The checks covered duplicate removal, missing-value handling, original row counts, and generation of all three CSV exports. Colab upload, download, and table display were simulated for that check.

This verifies the workflow on sample data; it does not establish findings for a particular customer dataset. Saved execution outputs are cleared in the distributed notebook. Run it with your CSV to generate the actual charts and insights.

## Interpretation and Limitations

- Spending summaries and correlations use the cleaned, outlier-capped values. Compare them with the original data before drawing business conclusions.
- IQR capping is applied broadly to numeric features except recognized identifiers. Review whether ratings, counts, or numeric category codes should be treated differently.
- Filling an entirely missing numeric column with zero is a fallback, not an estimated measurement. Review or remove such columns before interpreting them.
- Identifiers without the recognized naming pattern may enter the numeric analysis; rename or explicitly exclude them when adapting the notebook.
- Categorical charts are limited to 30 unique values to keep plots readable.
- Correlation does not establish causation, and the notebook does not perform causal inference or statistical significance testing.
- Scaling and imputation are fitted on the full dataset for descriptive analysis. For predictive modeling, split the data first and fit all preprocessing only on the training set.
- Exported CSVs do not include reusable fitted scaler objects.

## Possible Future Improvements

- Make the minimum dataset-size requirement configurable.
- Let users explicitly select feature, identifier, and outlier-treatment columns.
- Add comparisons between raw and processed statistics.
- Save charts automatically for reports and presentations.
- Export fitted preprocessing objects for reuse.
- Add a predictive model with separate training, validation, and test data when a suitable target and objective are defined.
