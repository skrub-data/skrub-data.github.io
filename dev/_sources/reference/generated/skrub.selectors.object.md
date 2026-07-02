# object

### skrub.selectors.object()

Select columns whose dtype is `object` (pandas) or `pl.Object` (polars).

#### SEE ALSO
[`string`](skrub.selectors.stringhtml.md#skrub.selectors.string)
: Select columns that have a String data type.

[`has_dtype`](skrub.selectors.has_dtypehtml.md#skrub.selectors.has_dtype)
: Select columns whose dtype matches one of the provided dtypes.

### Notes

Before pandas 3.0, columns containing only strings have the `object`
dtype and are selected. From pandas 3.0 onwards they have the `string`
dtype and are not. Use [`string()`](skrub.selectors.stringhtml.md#skrub.selectors.string) for
selecting strings.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     dict(
...         mixed=pd.Series(['A', 10]),
...         numeric=pd.Series([1, 2]),
...         string=pd.Series(['A', 'B']).convert_dtypes(),
...     )
... )
>>> df.dtypes
mixed       object
numeric      int64
string         ...
dtype: object
```

```pycon
>>> s.select(df, s.object())
  mixed
0     A
1    10
```

<!-- !! processed by numpydoc !! -->
