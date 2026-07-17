# cardinality_below

### skrub.selectors.cardinality_below(threshold)

Select columns whose cardinality (number of unique values) is (strictly)     below `threshold`.

This selector is useful for identifying low-cardinality (discrete) features for
categorical encoding or for finding ID-like columns with high cardinality to
encode them in specific ways.

* **Parameters:**
  **threshold**
  : Columns with fewer than this many unique values are selected.
    Null values do not count in the cardinality.

#### SEE ALSO
[`has_nulls`](skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls)
: Select columns that contain null values.

[`filter`](skrub.selectors.filterhtml.md#skrub.selectors.filter)
: Use for custom cardinality-based selection criteria.

### Notes

Missing values do not count as unique values for cardinality. For example,
a column with values `[1, 2, 2, None]` has a cardinality of 2.

If unique value counting fails for a column (e.g., due to unsupported data types),
the column is not selected.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     dict(
...         a1=[1, 1, 1, None],
...         a2=[1, 1, 2, None],
...         a2_b=[1, 1, 2, 2],
...         a3=[1, 2, 3, None],
...         a3_b=[1, 2, 3, 3],
...         a4=[1, 2, 3, 4],
...     )
... ).convert_dtypes()
>>> df
     a1    a2  a2_b    a3  a3_b  a4
0     1     1     1     1     1   1
1     1     1     1     2     2   2
2     1     2     2     3     3   3
3  <NA>  <NA>     2  <NA>     3   4
```

Select low-cardinality columns (e.g., below 3 unique values):

```pycon
>>> s.select(df, s.cardinality_below(3))
     a1    a2  a2_b
0     1     1     1
1     1     1     1
2     1     2     2
3  <NA>  <NA>     2
```

Invert to select high-cardinality columns (i.e., exclude low-cardinality):

```pycon
>>> s.select(df, ~s.cardinality_below(3))
    a3  a3_b  a4
0     1     1   1
1     2     2   2
2     3     3   3
3  <NA>     3   4
```

Select numeric features with low cardinality:

```pycon
>>> s.select(df, s.cardinality_below(10) & s.numeric())
    a1    a2  a2_b    a3  a3_b  a4
0     1     1     1     1     1   1
1     1     1     1     2     2   2
2     1     2     2     3     3   3
3  <NA>  <NA>     2  <NA>     3   4
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
