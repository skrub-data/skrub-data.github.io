# regex

### skrub.selectors.regex(pattern, flags=0)

Select columns by name with a regular expression.

Use this selector for complex name patterns that glob patterns cannot express.
This is useful for selecting columns with specific naming conventions or
patterns that glob patterns cannot express, so that regular expressions are
needed (e.g., columns matching `'^feature_[0-9]+$'`).
For simple wildcard patterns, consider [`glob()`](skrub.selectors.globhtml.md#skrub.selectors.glob).

* **Parameters:**
  **pattern**
  : A regular expression pattern to match column names. Can be a string pattern
    or a compiled regular expression object.

  **flags**
  : Regular expression flags as described in the `re` module documentation:
    [https://docs.python.org/3/library/re.html#flags](https://docs.python.org/3/library/re.html#flags)

#### SEE ALSO
[`glob`](skrub.selectors.globhtml.md#skrub.selectors.glob)
: Select columns by name with Unix shell-style wildcard patterns. Use this for simpler patterns.

[`filter_names`](skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names)
: Select columns based on custom name-based criteria.

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
```

Select columns matching a pattern:

```pycon
>>> s.select(df, s.regex('.*_mm'))
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

Use regex flags for case-insensitive matching (refer to the regex docs for
more detail):

```pycon
>>> import re
>>> s.select(df, s.regex('id', flags=re.I))
   ID
0   4
1   3
```

Combine with other selectors:

```pycon
>>> s.select(df, s.regex('^[a-z]+_mm$') | s.glob('ID'))
   height_mm  width_mm  ID
0      297.0     210.0   4
1      420.0     297.0   3
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
