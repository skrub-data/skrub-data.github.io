# cardinality_below

### skrub.selectors.cardinality_below(threshold)

Select columns whose cardinality (number of unique values) is (strictly)     below `threshold`.

Null values do not count in the cardinality.

#### SEE ALSO
[`has_nulls`](skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls)
: Select columns that contain null values.

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
>>> s.select(df, s.cardinality_below(3))
     a1    a2  a2_b
0     1     1     1
1     1     1     1
2     1     2     2
3  <NA>  <NA>     2
>>> s.select(df, s.cardinality_below(4))
     a1    a2  a2_b    a3  a3_b
0     1     1     1     1     1
1     1     1     1     2     2
2     1     2     2     3     3
3  <NA>  <NA>     2  <NA>     3
```

Invert the selector to select columns whose cardinality is above the threshold:
>>> s.select(df, ~s.cardinality_below(3))

> a3  a3_b  a4

0     1     1   1
1     2     2   2
2     3     3   3
3  <NA>     3   4

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
