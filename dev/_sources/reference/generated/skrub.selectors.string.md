# string

### skrub.selectors.string()

Select columns that have a string data type.

In pandas, object columns containing strings are also selected.

#### SEE ALSO
[`categorical`](skrub.selectors.categoricalhtml.md#skrub.selectors.categorical)
: Select categorical columns (explicit categories, not arbitrary strings).

[`object`](skrub.selectors.objecthtml.md#skrub.selectors.object)
: Select object dtype columns (broader, may include mixed types).

[`filter`](skrub.selectors.filterhtml.md#skrub.selectors.filter)
: Use for custom text-based selection criteria.

### Notes

#### WARNING
The behavior of string columns may change depending on the pandas version:

- Before pandas 3.0: String columns may have the ‘object’ dtype
- From pandas 3.0 onwards: String columns have only the ‘string’ dtype

This selector handles both cases, selecting string columns regardless of
pandas version. Object columns containing mixed types (e.g., strings and
numbers) are not selected.

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

Select all string columns (note: mixed-type object columns are excluded):

```pycon
>>> s.select(df, s.string())
object_string string
0             A      A
1             B      B
```

Combine with categorical() to select all text-like columns:

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
