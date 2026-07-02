# has_dtype

### skrub.selectors.has_dtype(\*dtypes)

Select columns whose dtype is equal to one of the provided dtypes.

This selector takes a hands-off approach: skrub does not normalize or infer
dtypes across dataframe libraries. A column is selected if
`sbd.dtype(column) == dtype` for at least one of the provided `dtypes`.

The most reliable way to use this selector is to get the dtype from an
existing column and pass that dtype object directly.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     {
...         "items": [["A4", "A3"], ["A5"]],
...         "count": [2, 1],
...     }
... )
>>> items_dtype = df["items"].dtype
>>> s.select(df, s.has_dtype(items_dtype))
       items
0  [A4, A3]
1      [A5]
```

Several dtypes can be accepted:

```pycon
>>> count_dtype = df["count"].dtype
>>> s.select(df, s.has_dtype(items_dtype, count_dtype))
       items  count
0  [A4, A3]      2
1      [A5]      1
```

Some dataframe libraries may also accept shorthand values that compare
equal to a dtype object. However, this is backend-specific, so the most
robust approach is still to pass the dtype obtained from the dataframe
library itself.

<!-- !! processed by numpydoc !! -->
