# Selector

### *class* skrub.selectors.Selector

Pattern for matching columns in a dataframe.

A `Selector` is a reusable rule for selecting columns based on various
criteria (data type, name pattern, content properties, etc.). Selectors
enable delayed selection: you can define a selection rule before the data
is available.

**How to use selectors**

- **Direct selection:** `s.select(df, selector)` returns a filtered dataframe
- **With** [`skrub.ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols): `ApplyToCols(transformer, cols=selector)`
  applies a transformer to selected columns
- **In DataOps:** `skrub.X(df).skb.apply(transformer, cols=selector)`
- **Manual expansion:** `selector.expand(df)` gets column names for manual use

**Combining Selectors**

Selectors can be combined with set operators to create complex selection rules:

- `s.numeric() | s.boolean()` - numeric OR boolean columns
- `s.all() - s.glob('*_id')` - all columns except those whose name ends with “_id”
- `~s.cardinality_below(10)` - high-cardinality columns

#### NOTE
This class is not meant to be instantiated manually. Create selectors using
builder functions such as [`skrub.selectors.all()`](skrub.selectors.allhtml.md#skrub.selectors.all),
[`skrub.selectors.cols()`](skrub.selectors.colshtml.md#skrub.selectors.cols), [`skrub.selectors.glob()`](skrub.selectors.globhtml.md#skrub.selectors.glob), etc.

#### SEE ALSO
[`skrub.ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols)
: Apply a transformer only to some columns, possibly selected by a selector.

[`skrub.DataOp.skb.apply`](skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply)
: Apply a transformer to selected columns in a DataOps workflow.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame({
...     'height_mm': [297.0, 420.0],
...     'width_mm': [210.0, 297.0],
...     'kind': ['A4', 'A3'],
...     'ID': [4, 3],
... })
```

Use a selector to get the names of matching columns:

```pycon
>>> s.numeric().expand(df)
['height_mm', 'width_mm', 'ID']
```

Selectors can be combined to create complex selection rules. For example,
to select numeric columns but exclude the ‘ID’ column:

```pycon
>>> (s.numeric() - 'ID').expand(df)
['height_mm', 'width_mm']
```

Use in data transformations, for example to apply a scaler only to numeric columns:

```pycon
>>> from skrub import ApplyToCols
>>> from sklearn.preprocessing import StandardScaler
>>> ApplyToCols(StandardScaler(), cols=s.numeric()).fit_transform(df)
kind  height_mm  width_mm   ID
0   A4       -1.0      -1.0  1.0
1   A3        1.0       1.0 -1.0
```

### Methods

| [`expand`](#skrub.selectors.Selector.expand)(df)             | Get the names of columns matched by the selector in a list.   |
|--------------------------------------------------------------|---------------------------------------------------------------|
| [`expand_index`](#skrub.selectors.Selector.expand_index)(df) | Get the indices of columns matched by the selector in a list. |
<!-- !! processed by numpydoc !! -->

#### expand(df)

Get the names of columns matched by the selector in a list.

This method evaluates the selector’s matching criteria against each column
in the dataframe and returns the names of columns that match. This can be
useful to extract column names to be used in dataframe operations.

* **Parameters:**
  **df**
  : A pandas or polars dataframe to evaluate the selector against.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The names of columns from `df` that the selector matches.

#### SEE ALSO
[`expand_index`](#skrub.selectors.Selector.expand_index)
: Get indices of matching columns instead of names.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import selectors as s
>>> some_selector = ~s.glob("*_mm")
>>> df = pd.DataFrame(
...     {
...         "height_mm": [210.0, 297.0],
...         "width_mm": [188.5, 210.0],
...         "kind": ["A5", "A4"],
...         "ID": [5, 4],
...     }
... )
>>> some_selector.expand(df)
['kind', 'ID']
```

Use to select columns in a dataframe:

```pycon
>>> df[some_selector.expand(df)]
   kind  ID
0   A5   5
1   A4   4
```

<!-- !! processed by numpydoc !! -->

#### expand_index(df)

Get the indices of columns matched by the selector in a list.

This method evaluates the selector against each column and returns the
positional indices (0, 1, 2, …) of matching columns instead of their names.

* **Parameters:**
  **df**
  : A pandas or polars dataframe to evaluate the selector against.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`int`](https://docs.python.org/3/library/functions.html#int)
  : The indices (zero-indexed) of columns from `df` that the
    selector matches.

#### SEE ALSO
[`expand`](#skrub.selectors.Selector.expand)
: Get names of matching columns instead of indices.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import selectors as s
>>> some_selector = ~s.glob("*_mm")
>>> df = pd.DataFrame(
...     {
...         "height_mm": [210.0, 297.0],
...         "width_mm": [188.5, 210.0],
...         "kind": ["A5", "A4"],
...         "ID": [5, 4],
...     }
... )
>>> some_selector.expand_index(df)
[2, 3]
```

<!-- !! processed by numpydoc !! -->
