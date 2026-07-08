# S&P 500 Linear Algebra Modeling Project

An educational Advanced Linear Algebra project exploring two modeling ideas on historical stock-price data: linear regression with rolling features and matrix factorization with truncated singular value decomposition (SVD).

This repository should be read as a coursework modeling exercise, not as a trading system and not as financial advice.

## Project Context

- Course: ES-205 / Advanced Linear Algebra semester project
- Team: Ahmed Musharaf, Muhammad Arsal, Wardah Haya
- Main tools explored: linear regression and matrix factorization
- Main dataset in the repository: `LinearRegression and Matrix Factorization/all_stocks_5yr.csv`

The reports in the repository discuss S&P 500 prediction in a broad educational framing. The code and data currently included use the `all_stocks_5yr.csv` dataset, which contains individual stock records from 2013 to 2018.

## Dataset Verified From Repository

`all_stocks_5yr.csv` contains:

| Field | Value |
| --- | --- |
| Rows | 619,040 |
| Stock symbols | 505 |
| Date range | 2013-02-08 to 2018-02-07 |
| Columns | `date`, `open`, `high`, `low`, `close`, `volume`, `Name` |

The dataset has missing values in some price columns, which the matrix factorization notebook handles with mean imputation.

## Repository Contents

```text
.
|-- README.md
|-- ES-205 DOCUMENTATION.pdf
|-- ES-205 DOCUMENTATION.docx
|-- ES-205 REPORT REMAKE GROUP 8.pdf
|-- ES-205 REPORT REMAKE GROUP 8.docx
|-- S&P 500 predictions with LR and MF (group 8).pdf
|-- scannable.jpg
`-- LinearRegression and Matrix Factorization/
    |-- all_stocks_5yr.csv
    |-- Linear Regression.ipynb
    `-- Matrix Factorization.ipynb
```

The PDF files are coursework/report artifacts. The notebooks contain the executable analysis.

## Evidence / Report

- [Primary project report PDF](S%26P%20500%20predictions%20with%20LR%20and%20MF%20%28group%208%29.pdf) - report artifact for the linear regression and matrix factorization project.
- [ES-205 documentation PDF](ES-205%20DOCUMENTATION.pdf) - coursework documentation artifact.
- [ES-205 report remake PDF](ES-205%20REPORT%20REMAKE%20GROUP%208.pdf) - alternate/report remake artifact retained in the repository.

## Linear Regression Notebook

File: `LinearRegression and Matrix Factorization/Linear Regression.ipynb`

The notebook:

1. Loads `all_stocks_5yr.csv`.
2. Drops the `Name` column.
3. Converts `date` to a datetime column.
4. Groups the data by date and sums numeric stock fields across symbols.
5. Sorts the dataset chronologically.
6. Creates rolling features based on prior data.
7. Trains a `sklearn.linear_model.LinearRegression` model.
8. Evaluates predictions with mean absolute error (MAE).
9. Plots actual test close values against predicted values.

Features used by the model:

```text
5_day_avg
30_day_avg
year_avg
avg_ratio
5_day_std
year_std
std_ratio
```

The saved notebook output reports an MAE of approximately `541.92` on the test split.

## Matrix Factorization Notebook

File: `LinearRegression and Matrix Factorization/Matrix Factorization.ipynb`

The notebook:

1. Loads `all_stocks_5yr.csv`.
2. Selects numerical columns: `open`, `high`, `low`, `close`, and `volume`.
3. Imputes missing values using `SimpleImputer(strategy="mean")`.
4. Standardizes the numeric columns using `StandardScaler`.
5. Applies `sklearn.decomposition.TruncatedSVD` with `n_components = 3`.
6. Reconstructs the standardized matrix from the reduced factors.
7. Plots the first three reconstructed components in a 3D scatter plot.
8. Prints the learned SVD component matrix.

This is matrix factorization through truncated SVD, not non-negative matrix factorization.

## Mathematical Concepts Practiced

- Least-squares regression
- Rolling feature construction for time-series data
- Matrix decomposition
- Dimensionality reduction
- Truncated singular value decomposition
- Data standardization before factorization
- Model evaluation with mean absolute error

## How to Run

There is no `requirements.txt` in the repository. Install the packages used by the notebooks manually:

```bash
python -m pip install pandas numpy matplotlib scikit-learn notebook
```

Start Jupyter Notebook from the repository root:

```bash
jupyter notebook
```

Open the notebooks inside:

```text
LinearRegression and Matrix Factorization/
```

Run `Linear Regression.ipynb` and `Matrix Factorization.ipynb` cell by cell.

## Important Limitations

- This is an educational modeling project, not an investment tool.
- The included CSV covers 2013-2018 individual stock records, while some report text references a broader 1950-2015 S&P 500 index history.
- The linear regression notebook groups by date and sums numeric fields across stock symbols, so the resulting `close` value is not the same as the official S&P 500 index close.
- The feature named `30_day_avg` is calculated with `rolling(window=5)` in the saved notebook, so it currently duplicates the 5-day rolling window logic.
- The feature named `year_std` is calculated with a rolling mean rather than a rolling standard deviation in the saved notebook.
- The model is evaluated with a simple train/test split and does not simulate a realistic walk-forward trading workflow.
- No transaction costs, slippage, risk controls, macroeconomic variables, or out-of-sample deployment checks are included.

## What I Learned

- How linear regression can be applied to lagged and rolling stock features.
- Why time-series data must avoid future leakage when creating predictors.
- How SVD can reduce a high-volume numeric dataset into a smaller set of components.
- Why financial modeling needs careful framing: a model can be useful for learning math and data processing without being reliable for real trading decisions.

## Future Improvements

- Correct the rolling-window feature definitions.
- Use official index-level data if the target is truly S&P 500 index prediction.
- Add a walk-forward validation loop.
- Compare linear regression with tree-based or regularized models.
- Add clear notebook markdown explaining each step and result.
- Add a `requirements.txt` file for reproducible setup.

## Disclaimer

This repository is for education only. It does not provide financial advice, investment recommendations, or a reliable market prediction system.
