# has_nulls

### skrub.selectors.has_nulls(proportion=0.0)

Select columns that contain at least one null value, or a proportion of null     values above a given threshold.

Use this selector to identify columns needing imputation or
with excessive missing data. This is useful for data quality
checks and preprocessing pipelines.

* **Parameters:**
  **proportion**
  : Default 0.0. Select columns where the proportion of null values exceeds
    this threshold (range: 0.0 to 1.0).
    - 0.0 (default): Selects any column with at least one null value
    - 0.5: Selects columns with >50% missing values
    - 1.0: Selects columns where all values are null

#### SEE ALSO
[`cardinality_below`](skrub.selectors.cardinality_belowhtml.md#skrub.selectors.cardinality_below)
: Select columns whose cardinality is below a threshold.

[`skrub.DropUninformative`](skrub.DropUninformativehtml.md#skrub.DropUninformative)
: Automatically drop columns that are uninformative, including columns with more null values than a specified threshold.

[`skrub.Cleaner`](skrub.Cleanerhtml.md#skrub.Cleaner)
: Parse common null representations (e.g., ‘NA’, ‘missing’) into proper null values, and possibly drop columns with excessive nulls.

[`filter`](skrub.selectors.filterhtml.md#skrub.selectors.filter)
: Use for custom null-based selection criteria.

### Notes

Null values include NaN, None, NA, etc., depending on the dataframe library.
Behavior:

- pandas: Recognizes np.nan, None, pd.NA, pd.NaT
- polars: Recognizes null values, and NaNs are treated as nulls.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(dict(a=[0, 1, 2], b=[0, None, 20], c=['a', 'b', None]))
```

Select all columns with at least one null value:

```pycon
>>> s.select(df, s.has_nulls())
      b     c
0   0.0     a
1   ...     b
2  20.0   ...
```

Select columns with >50% missing values:

```pycon
>>> df2 = pd.DataFrame(dict(
...     few_nulls=[1, 2, 3, None],
...     many_nulls=[1, None, None, None],
...     no_nulls=[1, 2, 3, 4]))
```

```pycon
>>> s.select(df2, s.has_nulls(proportion=0.5))
many_nulls
0        1.0
1        ...
2        ...
3        ...
```

Invert to select columns with NO null values:

```pycon
>>> s.select(df2, ~s.has_nulls())
no_nulls
0        1
1        2
2        3
3        4
```

Drop columns with >10% missing data:

```pycon
>>> from skrub import DropCols
>>> DropCols(cols=s.has_nulls(proportion=0.10)).fit_transform(df2)
no_nulls
0        1
1        2
2        3
3        4
```

<!-- !! processed by numpydoc !! -->
