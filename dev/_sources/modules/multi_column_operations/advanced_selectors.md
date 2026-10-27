<a id="user-guide-advanced-selectors"></a>

# [`filter()`](../../reference/generated/skrub.selectors.filterhtml.md#skrub.selectors.filter) and [`filter_names()`](../../reference/generated/skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names) to select with user-defined criteria

[`filter()`](../../reference/generated/skrub.selectors.filterhtml.md#skrub.selectors.filter) and [`filter_names()`](../../reference/generated/skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names) allow
selecting columns based on arbitrary user-defined criteria. These are also used to
implement many of the other selectors provided in this module.

[`filter()`](../../reference/generated/skrub.selectors.filterhtml.md#skrub.selectors.filter) accepts a function which will be called on a column
(i.e., a Pandas or polars Series). This function, called a predicate, must return
`True` if the column should be selected.

```pycon
>>> import pandas as pd
>>> import skrub.selectors as s
>>> df = pd.DataFrame(
...     {
...         "height_mm": [297.0, 420.0],
...         "width_mm": [210.0, 297.0],
...         "kind": ["A4", "A3"],
...         "ID": [4, 3],
...     }
... )
>>> s.select(df, s.filter(lambda col: "A4" in col.tolist()))
  kind
0   A4
1   A3
```

[`filter_names()`](../../reference/generated/skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names) accepts a predicate that is passed the column name,
instead of the column.

```pycon
>>> s.select(df, s.filter_names(lambda name: name.endswith('mm')))
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

We can pass args and kwargs that will be forwarded to the predicate, to help avoid
lambda or local functions and thus ensure the selector is picklable.

```pycon
>>> s.select(df, s.filter_names(str.endswith, 'mm'))
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

## Example of custom criteria in [`filter()`](../../reference/generated/skrub.selectors.filterhtml.md#skrub.selectors.filter): selecting columns with outliers

The [`filter()`](../../reference/generated/skrub.selectors.filterhtml.md#skrub.selectors.filter) selector can be used to select columns based on custom
criteria. For example, we can define a function that checks if a column contains
outliers using the Interquartile Range (IQR) method, and then use this function
with [`filter()`](../../reference/generated/skrub.selectors.filterhtml.md#skrub.selectors.filter) to select such columns.

Specifically, we define a function that computes the IQR (Inter Quartile Range) of a column
and checks if any data points extend further than 2 IQRs of the lower and upper quartile.

```pycon
>>> def has_outliers(column):
...    q1 = column.quantile(0.25)
...    q3 = column.quantile(0.75)
...    IQR = q3 - q1
...    lower_bound = q1 - 2 * IQR
...    upper_bound = q3 + 2 * IQR
...    outliers = (column < lower_bound) | (column > upper_bound)
...    return any(outliers)
```

```pycon
>>> from skrub import SelectCols
>>> select = SelectCols(s.filter(has_outliers))
>>> data = pd.DataFrame({
...     "A": [10, 12, 14, 15, 100],  # Outlier in column A
...     "B": [20, 22, 21, 19, 20],   # No outliers in column B
...     "C": [30, 29, 31, 32, 300]   # Outlier in column C
... })
>>> select.fit_transform(data)
     A    C
0   10   30
1   12   29
2   14   31
3   15   32
4  100  300
```

# Select columns with null values

Selectors [`has_nulls()`](../../reference/generated/skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls) and [Removing unneeded columns with DropUninformative and Cleaner](drop_uninformativehtml.md#user-guide-drop-uninformative) can be used to get information
about columns with null values. The selector [`has_nulls()`](../../reference/generated/skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls) selects columns that contain
null values and it accepts an optional `proportion` parameter that allows **selecting** columns
based on the proportion of null values they contain.

## Example: Selecting columns by null percentage with [`has_nulls()`](../../reference/generated/skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls)

The [`has_nulls()`](../../reference/generated/skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls) selector can filter columns based on their proportion of missing values.
This is useful for identifying columns that may need imputation or further investigation.

```pycon
>>> import pandas as pd
>>> import skrub.selectors as s
>>> from skrub import SelectCols
```

Create a dataset with varying amounts of missing data:

```pycon
>>> df = pd.DataFrame({
...     'patient_id': [1, 2, 3, 4, 5, 6, 7, 8],
...     'age': [25.0, 30.0, None, 45.0, 50.0, None, 60.0, 65.0],           # 25% nulls
...     'blood_pressure': [120, None, None, None, 140, None, None, 150],  # 62.5% nulls
...     'diagnosis': ['flu', 'cold', None, None, None, None, None, None], # 75% nulls
...     'treatment': ['med_A', 'med_B', 'med_C', 'med_D', 'med_E', 'med_F', 'med_G', 'med_H']  # no nulls
... })
```

Select columns with at least 25% missing values:

```pycon
>>> s.select(df, s.has_nulls(proportion=0.25))
   blood_pressure diagnosis
0           120.0       flu
1             NaN      cold
2             NaN       ...
3             NaN       ...
4           140.0       ...
5             NaN       ...
6             NaN       ...
7           150.0       ...
```
