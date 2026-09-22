# Diabetes Dataset -- Data Preprocessing

## Overview

This project performs data loading, inspection, cleaning, outlier
handling, feature engineering, and feature scaling on a diabetes dataset
using Python and Pandas.

The notebook used in this project is:

`Modue4Ass5Data_Preprocessing.ipynb`

## Dataset

The dataset is loaded from the following GitHub repository:

`https://raw.githubusercontent.com/Srinithi-python/Diabetes_DataSetMod4Ass5/refs/heads/main/diabetes.csv`

The dataset contains patient-related measurements and categorical
information such as `Gender` and `CLASS`.

### Categorical Columns

-   `Gender`
    -   `F` -- Female
    -   `M` -- Male
-   `CLASS`
    -   `N` -- No diabetes
    -   `P` -- Pre-diabetes
    -   `Y` -- Yes, Diabetes

## Technologies and Libraries

-   Python
-   Pandas
-   Matplotlib
-   Scikit-learn

## Data Preprocessing Workflow

### 1. Data Loading and Inspection

The dataset is loaded using Pandas and inspected using:

-   `head()`
-   `tail()`
-   `shape`
-   `columns`
-   `info()`
-   `dtypes`

This provides an initial understanding of the dataset structure, size,
columns, and data types.

### 2. Rename Columns

The following columns are renamed to make them more descriptive:

  Original Column   New Column
  ----------------- --------------
  `ID`              `Visit_ID`
  `No_Pation`       `Patient_ID`

An unwanted `Unnamed: 0` column is also removed when present.

### 3. Categorical Value Cleaning

The `Gender` column is cleaned by removing surrounding spaces and
converting values to uppercase.

The `CLASS` column is also checked and cleaned for surrounding spaces.

Unique values and value counts are examined to identify unexpected
categorical values.

### 4. Statistical Summary

A statistical summary is generated for numerical columns using:

-   Mean
-   Median
-   Minimum
-   Maximum
-   Standard deviation

This provides information about the distribution and range of numerical
variables.

### 5. Box Plot

A box plot is created for the numerical columns to visualize:

-   Distribution
-   Median
-   Interquartile range
-   Spread
-   Potential outliers

The notebook uses a Y-axis range of 0 to 10 for the displayed box plot.

### 6. Missing Value Identification

Missing values are identified for each column using `isnull().sum()`.

Only columns containing missing values are displayed in the
missing-value summary.

### 7. Missing Value Imputation

Missing values are imputed using different strategies:

-   Numerical columns → **Median**
-   Categorical columns → **Mode**

The median is used for numerical variables because it is less affected
by extreme values. The mode is used for categorical variables because it
represents the most frequently occurring category.

After imputation, missing values are checked again to verify the result.

### 8. Duplicate Handling

Duplicate rows are identified using `duplicated()`.

The duplicate rows are then removed using:

``` python
df.drop_duplicates(inplace=True)
```

### 9. Outlier Handling

Different outlier strategies are applied according to the requirements
of each group of variables.

#### AGE, HbA1c, and BMI

Outliers are identified using the **IQR method**, but they are retained.

These extreme values may represent genuine patient characteristics and
can contain clinically meaningful information. Removing them could
result in information loss.

#### Creatinine Ratio (Cr)

Values exceeding the **99.5th percentile** are excluded.

The threshold is calculated using:

``` python
df["Cr"].quantile(0.995)
```

No IQR or Z-score method is used for this filtering.

#### Urea

Values exceeding the **99.9th percentile** are excluded.

The threshold is calculated using:

``` python
df["Urea"].quantile(0.999)
```

No IQR or other outlier method is used for this filtering.

#### Lipid-Related Columns

The following lipid-related columns are processed:

-   `LDL`
-   `VLDL`
-   `HDL`
-   `TG`
-   `Chol`

Extreme outliers are removed using the **IQR method**.

For each column:

``` text
IQR = Q3 - Q1
Lower Limit = Q1 - 1.5 × IQR
Upper Limit = Q3 + 1.5 × IQR
```

Values outside these limits are removed.

### 10. Feature Engineering -- Gender Encoding

The `Gender` column is converted from categorical values into numerical
values using `LabelEncoder` from Scikit-learn.

A new column called:

`Gender_Encoded`

is created for model-building purposes.

Example:

``` text
Gender → Gender_Encoded
F      → 0
M      → 1
```

The exact numeric mapping is determined by the fitted `LabelEncoder`.

### 11. Feature Scaling

Standardization using `StandardScaler` is applied to the selected
numerical columns:

-   `Patient_ID`
-   `AGE`
-   `BMI`
-   `Cr`

Standardization transforms variables to a comparable scale using Z-score
scaling.

After scaling, the transformed features have approximately:

-   Mean = 0
-   Standard deviation = 1

Standardization was selected because the numerical variables have
different ranges. Bringing them to a common scale helps prevent
differences in magnitude from disproportionately affecting subsequent
statistical analysis or machine-learning models.

## Key Preprocessing Decisions

  -----------------------------------------------------------------------
  Data/Feature Group                  Treatment
  ----------------------------------- -----------------------------------
  `ID`                                Renamed to `Visit_ID`

  `No_Pation`                         Renamed to `Patient_ID`

  `Unnamed: 0`                        Removed when present

  `Gender`                            Cleaned and label encoded

  `CLASS`                             Checked and cleaned

  Numerical missing values            Median imputation

  Categorical missing values          Mode imputation

  Duplicate rows                      Removed

  `AGE`, `HbA1c`, `BMI` outliers      Identified and retained

  `Cr` extreme values                 Values above 99.5th percentile
                                      removed

  `Urea` extreme values               Values above 99.9th percentile
                                      removed

  `LDL`, `VLDL`, `HDL`, `TG`, `Chol`  IQR-based extreme outliers removed

  Selected numerical features         Standardized using `StandardScaler`
  -----------------------------------------------------------------------

## Insights

-   The dataset contains both numerical and categorical variables that
    require different preprocessing techniques.
-   Missing numerical and categorical values are handled using
    strategies appropriate to their data types.
-   Duplicate records are removed before further analysis.
-   Not all outliers are treated the same way; the preprocessing rules
    are applied according to the requirements of each feature.
-   `AGE`, `HbA1c`, and `BMI` outliers are preserved because they may
    represent genuine patient characteristics.
-   Very extreme values in `Cr` and `Urea` are filtered using the
    specified percentile thresholds.
-   Extreme values in lipid-related variables are handled using the IQR
    method.
-   Encoding `Gender` converts the categorical information into a
    numerical representation suitable for machine-learning workflows.
-   Standardization places selected numerical variables on a comparable
    scale.

## How to Run

1.  Install the required libraries:

``` bash
pip install pandas matplotlib scikit-learn
```

2.  Open `Modue4Ass5Data_Preprocessing.ipynb` in Jupyter Notebook,
    JupyterLab, Google Colab, or VS Code.

3.  Run the notebook cells in order.

4.  The dataset is loaded directly from the GitHub URL included in the
    notebook.

## Project Structure

``` text
Diabetes_DataSetMod4Ass5/
│
├── Modue4Ass5Data_Preprocessing.ipynb
└── README.md
```

## Conclusion

This project demonstrates a complete data-preprocessing workflow for a
diabetes dataset. It covers data inspection, column renaming,
categorical cleaning, statistical analysis, missing-value imputation,
duplicate removal, feature-specific outlier handling, categorical
encoding, and numerical feature scaling. The processed dataset can
subsequently be used for statistical analysis or machine-learning model
development.
