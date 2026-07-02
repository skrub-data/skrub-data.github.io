# filter

### skrub.selectors.filter(predicate, \*args, \*\*kwargs)

Select columns for which `predicate` returns True.

For each column `col` in the dataframe, `predicate` is called as
`predicate(col, *args, **kwargs)` and the column is kept if it returns
True. To filter columns based only on their name, see also
`filter_names`.

`args` and `kwargs` are extra parameters for the predicate. Storing
parameters like this rather than in a closure can help using an importable
function as the predicate rather than a local one, which is necessary to
pickle the selector. (An alternative is to use `functools.partial`).

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     {
...         "height_mm": [297.0, 420.0],
...         "width_mm": [210.0, 297.0],
...         "kind": ["A4", "A3"],
...         "ID": [4, 3],
...     }
... )
>>> df
   height_mm  width_mm kind  ID
0      297.0     210.0   A4   4
1      420.0     297.0   A3   3
```

```pycon
>>> selector = s.filter(lambda col: 'A4' in col.values)
>>> s.select(df, selector)
  kind
0   A4
1   A3
```

```pycon
>>> def contains(col, value):
...    return value in col.values
```

```pycon
>>> selector = s.filter(contains, 3)
>>> selector
filter(contains, 3)
```

```pycon
>>> s.select(df, selector)
   ID
0   4
1   3
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
