# inv

### skrub.selectors.inv(obj)

Invert a selector.

This selects all columns except those that are matched by the input; it is
equivalent to `all() - obj` or `~make_selector(obj)`. The argument
`obj` can be a selector but also a column name or list of column names.

* **Returns:**
  Selector
  : A `Selector` that is the inverse of the input selector.

#### SEE ALSO
[`all`](skrub.selectors.allhtml.md#skrub.selectors.all)
: Select all columns

### Notes

`inv` is primarily a convenience function. When working with a single
selector or column name, `~selector` may be more readable. When excluding
from the full set of columns, `all() - selector` is more explicit.

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
>>> s.select(df, ['ID'])
   ID
0   4
1   3
```

```pycon
>>> s.select(df, s.inv(['ID']))
   height_mm  width_mm kind
0      297.0     210.0   A4
1      420.0     297.0   A3
```

```pycon
>>> s.select(df, ~s.cols('ID'))
   height_mm  width_mm kind
0      297.0     210.0   A4
1      420.0     297.0   A3
```

<!-- !! processed by numpydoc !! -->
