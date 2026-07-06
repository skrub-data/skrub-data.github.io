# cols

### skrub.selectors.cols(\*columns)

Select columns by name.

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
>>> s.select(df, s.cols('height_mm', 'ID'))
   height_mm  ID
0      297.0   4
1      420.0   3
```

When this selector is used on its own, an error is raised if some columns
are missing:

```pycon
>>> s.select(df, s.cols('width_mm', 'depth_mm'))
Traceback (most recent call last):
    ...
ValueError: The following columns are requested for selection but missing from dataframe: ['depth_mm']
```

However, no error is raised when this selector is combined with other
selectors:

```pycon
>>> s.select(df, s.all() & s.cols('width_mm', 'depth_mm'))
   width_mm
0     210.0
1     297.0
```

In all skrub functions that accept a selector, a list of column names can
be passed and `cols` will be used to turn it into a selector.

```pycon
>>> s.select(df, ['kind', 'ID'])
  kind  ID
0   A4   4
1   A3   3
>>> s.make_selector(['kind', 'ID'])
cols('kind', 'ID')
>>> s.all() & ['kind', 'ID']
(all() & cols('kind', 'ID'))
```

<!-- !! processed by numpydoc !! -->
