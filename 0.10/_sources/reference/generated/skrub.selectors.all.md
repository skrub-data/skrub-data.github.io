# all

### skrub.selectors.all()

Select all columns in a dataframe.

#### SEE ALSO
[`inv`](skrub.selectors.invhtml.md#skrub.selectors.inv)
: Invert a selector

[`cols`](skrub.selectors.colshtml.md#skrub.selectors.cols)
: Select columns by exact name

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

Select all columns:

```pycon
>>> s.select(df, s.all())
   height_mm  width_mm kind  ID
0      297.0     210.0   A4   4
1      420.0     297.0   A3   3
```

Use `all()` as a base for excluding columns, for example by name:

```pycon
>>> s.select(df, s.all() - 'ID')
   height_mm  width_mm kind
0      297.0     210.0   A4
1      420.0     297.0   A3
```

<!-- !! processed by numpydoc !! -->
