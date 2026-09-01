# object

### skrub.selectors.object()

Select columns whose dtype is `object` (pandas) or `pl.Object` (polars).

Note that object columns may contain mixed types (e.g., strings and numbers) and are
broader than string columns. Use this selector when you specifically need
object-typed columns, and prefer more specific selectors like [`string()`](skrub.selectors.stringhtml.md#skrub.selectors.string)
or [`categorical()`](skrub.selectors.categoricalhtml.md#skrub.selectors.categorical).

#### SEE ALSO
[`string`](skrub.selectors.stringhtml.md#skrub.selectors.string)
: Select string columns (preferred for text data). Use this instead of object() for text columns.

[`categorical`](skrub.selectors.categoricalhtml.md#skrub.selectors.categorical)
: Select categorical columns.

[`has_dtype`](skrub.selectors.has_dtypehtml.md#skrub.selectors.has_dtype)
: Select columns whose dtype matches specific dtypes.

### Notes

#### WARNING
The behavior of string columns may change depending on the pandas version:

- Before pandas 3.0: String columns may have the `object` dtype
- From pandas 3.0 onwards: String columns have only the `string` dtype

This selector selects **all** `object` dtype columns regardless of content,
including mixed-type columns. For text data, prefer [`string()`](skrub.selectors.stringhtml.md#skrub.selectors.string) which is
more selective.

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

Select object dtype columns (note: can contain mixed types):

```pycon
>>> s.select(df, s.object())
  mixed
0     A
1    10
```

Prefer string() for text columns:

```pycon
>>> s.select(df, s.string())
  string
0      A
1      B
```

<!-- !! processed by numpydoc !! -->
