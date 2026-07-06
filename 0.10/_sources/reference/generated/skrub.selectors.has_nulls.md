# has_nulls

### skrub.selectors.has_nulls(proportion=0.0)

Select columns that contain at least one null value, or a proportion of null     values above a given threshold.

#### SEE ALSO
[`cardinality_below`](skrub.selectors.cardinality_belowhtml.md#skrub.selectors.cardinality_below)
: Select columns whose cardinality is below a threshold.

[`skrub.DropUninformative`](skrub.DropUninformativehtml.md#skrub.DropUninformative)
: Automatically drop columns that are uninformative, including columns with a fraction of null values above a given threshold.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(dict(a=[0, 1, 2], b=[0, None, 20], c=['a', 'b', None]))
>>> s.select(df, s.has_nulls())
      b     c
0   0.0     a
1   ...     b
2  20.0  ...
```

Use the `proportion` parameter to filter columns by null percentage:

```pycon
>>> df2 = pd.DataFrame(dict(
...     few_nulls=[1, 2, 3, None],
...     many_nulls=[1, None, None, None],
...     no_nulls=[1, 2, 3, 4]))
>>> s.select(df2, s.has_nulls(proportion=0.2))
   few_nulls  many_nulls
0        1.0         1.0
1        2.0         ...
2        3.0         ...
3        ...         ...
```

```pycon
>>> s.select(df2, s.has_nulls(proportion=0.5))
many_nulls
0        1.0
1        ...
2        ...
3        ...
```

<!-- !! processed by numpydoc !! -->
