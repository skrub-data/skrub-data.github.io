# regex

### skrub.selectors.regex(pattern, flags=0)

Select columns by name with a regular expression.

pattern can be a string pattern or a compiled regular expression, and flags
are regular expression flags as described in the `re` module
documentation:

[https://docs.python.org/3/library/re.html#flags](https://docs.python.org/3/library/re.html#flags)

#### SEE ALSO
[`glob`](skrub.selectors.globhtml.md#skrub.selectors.glob)
: Select columns by name with a Unix shell-style glob pattern.

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
>>> s.select(df, s.regex('.*_mm'))
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

A column is selected if `re.match(col_name, pattern, flags)` returns a
match. Note that it is enough to match at the beginning of the string:

```pycon
>>> s.select(df, s.regex('wid'))
   width_mm
0     210.0
1     297.0
```

Use ‘$’ to require matching until the end of the column name:

```pycon
>>> s.select(df, s.regex('wid$'))
Empty DataFrame
Columns: []
Index: [0, 1]
```

Flags are passed to `re.match`; the following are 3 equivalent ways of
setting re flags (re.IGNORECASE in this example):

```pycon
>>> import re
>>> s.select(df, s.regex('id', flags=re.I))
   ID
0   4
1   3
>>> s.select(df, s.regex('(?i)id'))
   ID
0   4
1   3
>>> s.select(df, s.regex(re.compile('id', re.I)))
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
