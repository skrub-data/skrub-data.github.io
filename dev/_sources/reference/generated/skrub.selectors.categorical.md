# categorical

### skrub.selectors.categorical()

Select columns that have a Categorical (or polars Enum) data type.

#### SEE ALSO
[`string`](skrub.selectors.stringhtml.md#skrub.selectors.string)
: Select string columns.

[`cardinality_below`](skrub.selectors.cardinality_belowhtml.md#skrub.selectors.cardinality_below)
: Select columns with low cardinality (low number of unique values).

[`skrub.ToCategorical`](skrub.ToCategoricalhtml.md#skrub.ToCategorical)
: Convert a column to categorical type for explicit category handling.

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

Select only categorical columns (note: string columns are not selected):

```pycon
>>> s.select(df, s.categorical())
  category
0        A
1        B
```

Combine with [`string()`](skrub.selectors.stringhtml.md#skrub.selectors.string) to select all text-like columns:

```pycon
>>> s.select(df, s.categorical() | s.string())
  string category
0      A        A
1      B        B
```

<!-- !! processed by numpydoc !! -->
