# Selector

### *class* skrub.selectors.Selector

Generic selector type, that returns set columns when applied.

This class is not meant to be instantiated manually, `Selector`
objects are created by calling one of the selector builders such
as [`skrub.selectors.all()`](skrub.selectors.allhtml.md#skrub.selectors.all) or [`skrub.selectors.make_selector()`](skrub.selectors.make_selectorhtml.md#skrub.selectors.make_selector).

### Methods

| [`expand`](#skrub.selectors.Selector.expand)(df)             | Lists the column names that the selector would retain if applied to         the dataframe `df`.          |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| [`expand_index`](#skrub.selectors.Selector.expand_index)(df) | Lists the indices of dataframe `df`'s columns that the selector         would retain if applied to `df`. |
<!-- !! processed by numpydoc !! -->

#### expand(df)

Lists the column names that the selector would retain if applied to         the dataframe `df`.

* **Parameters:**
  **df**
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list)
  : The list of `df`’s columns that the item would select.

### Notes

In effect, running `df[sel.expand(df)]` should give the exact same result as
`sel.transform(df)`.

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

<!-- !! processed by numpydoc !! -->

#### expand_index(df)

Lists the indices of dataframe `df`’s columns that the selector         would retain if applied to `df`.

* **Parameters:**
  **df**
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list)
  : The list of indices among `df`’s columns that the item would select.

### Notes

In effect, (as with `expand`), if `cols` is the list of columns in `df`,
running `df[cols[i] for i in sel.expand(df)]` should give the exact same
result as `sel.transform(df)`.

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
