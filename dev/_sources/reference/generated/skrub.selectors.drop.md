# drop

### skrub.selectors.drop(df, selector)

Select the columns of a dataframe that are NOT matched by the selector.

This is the complement of `select()`: it returns a new dataframe containing
all columns except those matched by the selector.

* **Parameters:**
  **df**
  : The dataframe to process.

  **selector**
  : A selector object, single column name, or list of column names indicating
    which columns to drop.
* **Returns:**
  dataframe
  : A new dataframe with the matched columns removed, preserving the order
    of remaining columns.

#### SEE ALSO
[`select`](skrub.selectors.selecthtml.md#skrub.selectors.select)
: Return only the columns matched by a selector

[`inv`](skrub.selectors.invhtml.md#skrub.selectors.inv)
: Create an inverted selector matching all columns except those from the input

### Notes

`drop` is logically equivalent to `select(df, ~selector)` or
`select(df, s.all() - selector)`.

`drop` preserves the original column order of the remaining columns.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     {
...         "height_mm": [210.0, 297.0],
...         "width_mm": [188.5, 210.0],
...         "kind": ["A5", "A4"],
...         "ID": [5, 4],
...     }
... )
>>> df
   height_mm  width_mm kind  ID
0      210.0     188.5   A5   5
1      297.0     210.0   A4   4
```

Drop columns matching a pattern:

```pycon
>>> s.drop(df, s.glob("*_mm"))
  kind  ID
0   A5   5
1   A4   4
```

Drop specific columns by name (can pass names directly):

```pycon
>>> s.drop(df, ['height_mm', 'width_mm'])
  kind  ID
0   A5   5
1   A4   4
```

Preserve only certain types (via drop):

```pycon
>>> s.drop(df, s.all() - s.string())
  kind
0   A5
1   A4
```

<!-- !! processed by numpydoc !! -->
