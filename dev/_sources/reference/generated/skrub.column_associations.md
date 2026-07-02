# column_associations

### skrub.column_associations(df)

Get measures of statistical associations between all pairs of columns.

Reported metrics include Cramer’s V statistic and Pearson’s Correlation
Coefficient. The result is returned as a dataframe that contains the column
name and idx for the left and right table and both associations; results
are sorted in descending order by Cramer’s V association.

* **Parameters:**
  **df**
  : The dataframe whose columns will be compared to each other.
* **Returns:**
  dataframe
  : The computed associations.

### Notes

The result is returned as a dataframe with columns:

`['left_column_name', 'left_column_idx', 'right_column_name',
'right_column_idx', 'cramer_v', 'pearson_corr']`

As the function is commutative, each pair of columns appears only once
(either `col_1`, `col_2` or `col_2`, `col_1` but not both).
The results are sorted from most associated to least associated.

To compute the Cramer’s V statistic, all columns are discretized. Numeric
columns are binned with 10 bins. For categorical columns, only the 10 most
frequent categories are considered. In both cases, nulls are treated as a
separate category, ie a separate row in the contingency table. Thus,
associations between the values of 2 columns or between their missingness
patterns may be captured.
Cramér’s V is a measure of association between two nominal variables,
giving a value between 0 and +1 (inclusive).

* [Cramer’s V](https://en.wikipedia.org/wiki/Cramér%27s_V)

To compute the Pearson’s Correlation Coefficient, only numeric columns are
considered. The correlation is computed using the Pearson method used in
pandas or polars, depending on the dataframe. In both cases, lines containing NaNs
are dropped.
Pearson’s Correlation Coefficient is a measure of the linear correlation
between two variables, giving a value between -1 and +1 (inclusive).

* [Pearson’s Correlation Coefficient](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)
* [pandas.DataFrame.corr](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.corr.html)

### Examples

```pycon
>>> import numpy as np
>>> import pandas as pd
>>> import skrub
>>> pd.set_option('display.width', 200)
>>> pd.set_option('display.max_columns', 10)
>>> pd.set_option('display.precision', 4)
>>> rng = np.random.default_rng(33)
>>> df = pd.DataFrame({f"c_{i}": rng.random(size=20)*10 for i in range(5)})
>>> df["c_str"] = [f"val {i}" for i in range(df.shape[0])]
>>> df.shape
(20, 6)
>>> df.head()
      c_0     c_1     c_2     c_3     c_4  c_str
0  4.4364  4.0114  6.9271  7.0970  4.8913  val 0
1  5.6849  0.7192  7.6430  4.6441  2.5116  val 1
2  9.0810  9.4011  1.9257  5.7429  6.2358  val 2
3  2.5425  2.9678  9.7801  9.9879  6.0709  val 3
4  5.8878  9.3223  5.3840  7.2006  2.1494  val 4
>>> # Compute the associations
>>> associations = skrub.column_associations(df)
>>> associations
   left_column_name  left_column_idx right_column_name  right_column_idx  cramer_v  pearson_corr
0              c_1                1               c_4                 4    0.8215        0.1597
1              c_0                0               c_1                 1    0.8215        0.1123
2              c_0                0               c_3                 3    0.7551        0.3212
3              c_1                1               c_3                 3    0.6837       -0.1887
4              c_0                0               c_4                 4    0.6837       -0.3202
5              c_3                3               c_4                 4    0.6053       -0.0150
6              c_2                2               c_3                 3    0.6053        0.1757
7              c_0                0               c_2                 2    0.6053       -0.0578
8              c_2                2               c_4                 4    0.5169       -0.2885
9              c_1                1               c_2                 2    0.4122       -0.4986
>>> pd.reset_option('display.width')
>>> pd.reset_option('display.max_columns')
>>> pd.reset_option('display.precision')
```

<!-- !! processed by numpydoc !! -->
