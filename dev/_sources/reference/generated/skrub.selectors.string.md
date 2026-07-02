# string

### skrub.selectors.string()

Select columns that have a String data type.

In pandas, object columns containing (only) strings are also selected.

#### SEE ALSO
[`categorical`](skrub.selectors.categoricalhtml.md#skrub.selectors.categorical)
: Select categorical columns.

### Notes

The behavior of string columns may change depending
on the major version of pandas: before pandas 3.0, string columns would have
the ‘object’ dtype, and after pandas 3.0 they have the ‘string’ dtype. This
selector is designed to select string columns in both cases, even if a column
has both the ‘object’ and ‘string’ dtype. If a column has only the ‘object’
dtype (e.g., it contains both strings and numbers), then it will not be selected.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     dict(
...         object_string=pd.Series(['A', 'B']),
...         object=pd.Series(['A', 10]),
...         string=pd.Series(['A', 'B']).convert_dtypes(),
...         categorical=pd.Series(['A', 'B'], dtype="category"),
...     )
... )
>>> df
object_string object string categorical
0             A      A      A           A
1             B     10      B           B
```

```pycon
>>> df.dtypes
object_string         ...
object             object
string                ...
categorical      category
dtype: object
```

Both the ‘object_string’ and ‘string’ columns are selected, but not the ‘object’
column. Categorical columns are not selected.

```pycon
>>> s.select(df, s.string())
object_string string
0             A      A
1             B      B
```

To select categorical columns as well, use the bitwise OR operator to combine
`s.string()` with [`categorical()`](skrub.selectors.categoricalhtml.md#skrub.selectors.categorical):

```pycon
>>> s.select(df, s.string() | s.categorical())
object_string string categorical
0             A      A           A
1             B      B           B
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
