# filter_names

### skrub.selectors.filter_names(predicate, \*args, \*\*kwargs)

Select columns based on their name.

For a column whose name is `col_name`, `predicate` is called as
`predicate(col_name, *args, **kwargs)` and the column is selected if
returns `True`. Note this is different from `filter`, because here the
predicate is passed the column name; with `filter`, the predicate
is passed the actual column (pandas or polars Series).

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
>>> selector = s.filter_names(lambda name: name.endswith('_mm'))
>>> s.select(df, selector)
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

If we want to pickle the selector, we’re better off using an importable
function and passing the arguments separately:

```pycon
>>> selector = s.filter_names(str.endswith, '_mm')
>>> selector
filter_names(str.endswith, '_mm')
```

```pycon
>>> s.select(df, selector)
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

```pycon
>>> import pickle
>>> _ = pickle.dumps(selector) # OK
```

<!-- !! processed by numpydoc !! -->
