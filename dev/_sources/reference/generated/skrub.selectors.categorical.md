# categorical

### skrub.selectors.categorical()

Select columns that have a Categorical (or polars Enum) data type.

#### SEE ALSO
[`string`](skrub.selectors.stringhtml.md#skrub.selectors.string)
: Select string columns.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     dict(
...         string=pd.Series(['A', 'B']),
...         category=pd.Series(['A', 'B'], dtype="category"),
...     )
... )
```

```pycon
>>> df
  string category
0      A        A
1      B        B
```

```pycon
>>> df.dtypes
string        ...
category    category
dtype: object
```

```pycon
>>> s.select(df, s.categorical())
  category
0        A
1        B
```

<!-- !! processed by numpydoc !! -->
